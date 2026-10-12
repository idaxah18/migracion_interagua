# 6.3 - Alta Facturación - Registrar

Formaliza la creación del reclamo por alta facturación ante los sistemas de Interagua. Valida la disponibilidad del teléfono de contacto, recibe observaciones libres del cliente, prepara la auditoría de registro, invoca el servicio de facturación, y entrega el comprobante oficial con su número de solicitud y tiempo máximo de respuesta reglamentario.

## Datos que debe traer al entrar

- `numeroContrato`: Código del contrato objeto del reclamo.
- `codigoCliente`, `codigoAplicante`, `codigoDireccionContrato`: Datos maestros obtenidos en el flujo anterior.
- `listaFactura`: Estructura JSON con las facturas reclamadas y sus respectivos cargos técnicos.
- `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 1. Verificación de teléfono de contacto

El sistema revisa si el usuario posee un número celular en su sesión:
- **Sin teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para capturarlo. Si el usuario no lo ingresa:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El flujo se interrumpe de inmediato).*
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

## Paso 3. Captura de observaciones del cliente

El bot solicita comentarios adicionales sobre el reclamo:

> Para finalizar, si tienes alguna observación escríbela por favor.

Entrada del usuario:
- Se almacena en la variable `comentario`.
- Si el usuario envía audios, stickers o archivos adjuntos en lugar de texto:
  > Debes ingresar únicamente texto para tu observación. Por favor, inténtalo nuevamente.

## Paso 4. Preparación de auditoría

El sistema formatea los campos requeridos por el servicio de facturación:
- `comentario`: Formateado como `WH <telefonoUsuario> <comentario recortado a 150 caracteres>`.
- `fechaRegistroSolicitud`: Fecha del día actual en zona horaria local (`YYYY-MM-DD`).

## Paso 5. Registro formal del reclamo en el Web Service

Servicio: **Registro Reclamos Facturación** (Tool ID `3009`)  
Endpoint: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "numeroContrato": "<numeroContrato>",
  "codigoCliente": "<codigoCliente>",
  "codigoAplicante": "<codigoAplicante>",
  "codigoDireccionContrato": "<codigoDireccionContrato>",
  "facturas": "<listaFactura>",
  "comentario": "<comentario>",
  "fechaRegistroSolicitud": "<fechaRegistroSolicitud>"
}
```

Evaluación de la respuesta:
1. **Errores de negocio del backend:** Si el servicio rechaza la creación debido a una condición comercial preexistente (ej. reclamo duplicado o periodo cerrado):
   - Se guardan `codigoError` y `DescripcionError`.
   - Se registra el marcador analítico `EP Alta Fact`.
   - Se deriva a [_ 6.4 - Alta Facturación - Mensaje Personalizado](./6-4-alta-facturacion-mensaje-personalizado.md) (Skill ID `51093`).
2. **Sin identificador de respuesta:** Si el servicio no retorna datos:  
   Mensaje: `Estimado cliente no hemos podido registrar tu reclamo.`
3. **Falla de conexión o servicio:**  
   Mensaje: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`  
   Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md).

Si la creación es exitosa:
- Se extrae el número de trámite `REQUEST_ID` en `numeroSolicitud`.

## Paso 6. Entrega de confirmación oficial

El bot envía el mensaje formal de culminación:

> Reclamo registrado con éxito.  
> El número de solicitud es {{$memory.numeroSolicitud}} y será atendido en un plazo máximo de 30 días.  
>   
> La respuesta le llegará a su correo electrónico.  
>   
> Para consultar el estado del trámite, ingresa al módulo *"Consulta de Solicitudes"* desde el menú principal o escribiendo la palabra *"Solicitudes"*.

## Salida

- **Trámite finalizado exitosamente:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Errores de negocio específicos:** Transfiere a [_ 6.4 - Alta Facturación - Mensaje Personalizado](./6-4-alta-facturacion-mensaje-personalizado.md) (Skill ID `51093`).
- **Fallas técnicas o caídas de servicio:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
