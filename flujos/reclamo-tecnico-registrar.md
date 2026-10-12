# Reclamo Técnico - Registrar

Formaliza el registro del reclamo por daño a producto en la infraestructura de agua o saneamiento. Valida la existencia de número telefónico de contacto, recibe y filtra las observaciones del cliente (bloqueando enlaces e inyecciones), invoca el servicio `Register Dano Producto`, actualiza la bitácora operativa en base de datos y entrega el comprobante oficial con el número de solicitud.

## Datos que debe traer al entrar

- `codigoProducto`, `codigoCliente`, `codigoDireccion`, `codigoDireccionProducto`: Datos maestros obtenidos en el flujo anterior.
- `codigoCausal`: Código de causal asignado a la avería.
- `tituloServicio`: Nombre descriptivo del daño seleccionado.
- `numeroContrato`: Código del contrato consultado.
- `identificacion`: Identificación del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `log_reclamo._id`: Identificador del registro en la tabla `9958`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 1. Verificación de contacto telefónico

El sistema revisa la presencia del teléfono en la sesión:
- **Sin teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para capturarlo. Si el usuario no lo ingresa:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El trámite se suspende).*
- **Con teléfono:** Continúa al Paso 2.

## Paso 2. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- En mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Operativo: Continúa al Paso 3.

## Paso 3. Captura y filtrado de observaciones

El bot solicita detalles sobre el daño reportado:

> Para finalizar, si tienes alguna observación escríbela por favor.

Entrada y validaciones regex:
1. Solo se admiten caracteres alfanuméricos y puntuación común (`^[a-zA-ZáéíóúÁÉÍÓÚñÑ0-9\s.,;:()\-_/°"#%&¡!¿?@]+$`).
2. Se rechaza estrictamente cualquier enlace web o URL (`\b(?:https?:\/\/|www\.)[^\s<>"]+\b`).
3. Si el mensaje infringe estas restricciones:
   > Debes ingresar únicamente texto para tu observación. Por favor, inténtalo nuevamente.

Al superar la validación, se formatea la variable para auditoría:
`comentario = "WH " + telefonoUsuario + " " + comentario`

## Paso 4. Registro formal del daño a producto

Servicio: **Register Dano Producto** (Tool ID `2941`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "url": "{{$memory.urlService}}",
  "comentario": "<comentario>",
  "codigoCausal": "<codigoCausal>",
  "codigoCliente": "<codigoCliente>",
  "codigoProducto": "<codigoProducto>",
  "identificacion": "<identificacion>",
  "codigoDireccion": "<codigoDireccion>",
  "codigoDireccionProducto": "<codigoDireccionProducto>"
}
```

Evaluación de la respuesta:
1. **Errores de negocio devueltos por el Web Service (`mappingError`):**  
   Si el servicio rechaza el registro por condiciones previas de la cuenta (solicitud duplicada, suspensión o falta de servicio activo), almacena `codigoError` y `DescripcionError`, actualiza la base de datos y transfiere a [_ Reclamo Técnico - Mensaje Personalizado](./reclamo-tecnico-mensaje-personalizado.md) (Skill ID `51187`).
2. **Sin paquete devuelto:** Si no hay datos:  
   Mensaje: `Estimado cliente no hemos podido registrar tu reclamo.`
3. **Falla de conexión o error general:**  
   Mensaje: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`  
   Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md).

Si es exitoso:
- Se captura el código de trámite `PACKAGE_ID` en `numeroSolicitud`.

## Paso 5. Cierre de bitácora en base de datos

Se actualiza el registro en la tabla `9958` (`log_reclamo`):

Operación: **Update**  
Tabla ID: `9958`  
Filtro: `id: "{{$memory.log_reclamo._id}}"`  
Datos actualizados:
```json
{
  "estado": "registrado",
  "fecha_fin": "{{$context.fecha_hora_fin}}"
}
```

## Paso 6. Entrega de confirmación oficial

El bot envía el mensaje formal de ingreso:

> Tu número de Solicitud {{$memory.numeroSolicitud}} por {{$memory.tituloServicio}} ha sido ingresado exitosamente 👍

## Salida

- **Trámite completado con éxito:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Errores de negocio de backend:** Transfiere a [_ Reclamo Técnico - Mensaje Personalizado](./reclamo-tecnico-mensaje-personalizado.md) (Skill ID `51187`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
