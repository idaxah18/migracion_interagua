# Reclamo Técnico - Incidentes

Registra incidentes operativos y eventualidades del servicio técnico en el sistema comercial de Interagua. Solicita y normaliza las observaciones del cliente (depurando tildes y caracteres no alfabéticos), invoca el servicio `Register Incidente`, y entrega la constancia oficial con el número de trámite.

## Datos que debe traer al entrar

- `codigoProducto`, `codigoCliente`, `codigoDireccion`, `codigoDireccionProducto`: Identificadores maestros del contrato y del predio.
- `codigoCausal`: Código técnico asignado a la causal del incidente.
- `tituloServicio`: Descripción amigable del incidente reportado.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- En mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Operativo: Continúa al Paso 2.

## Paso 2. Captura y normalización de observaciones

El bot solicita comentarios adicionales:

> Para finalizar, si tienes alguna observación escríbela por favor.

Tratamiento de texto:
1. Se remueven diacríticos y acentos ortográficos.
2. Se filtran caracteres especiales dejando únicamente letras y espacios.
3. Se prefija el canal y teléfono: `WH <telefonoUsuario> <comentario_limpio>`.

## Paso 3. Registro formal del incidente

Servicio: **Register Incidente** (Tool ID `2954`)  
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
1. **Solicitud preexistente:**  
   - Código `903034`: `Actualmente ya cuentas con una solicitud en estado registrado por el mismo concepto.`  
   Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`).
2. **Sin paquete devuelto:**  
   Mensaje: `Estimado cliente no hemos podido registrar tu incidente.`
3. **Falla de conexión o caída de servicio:**  
   Mensaje: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`  
   Deriva a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).

Si es exitoso:
- Se obtiene el código `PACKAGE_ID` en `numeroSolicitud`.

## Paso 4. Entrega de confirmación oficial

El bot envía el comprobante de registro:

> Tu número de Solicitud {{$memory.numeroSolicitud}} por {{$memory.tituloServicio}} ha sido ingresado exitosamente 👍

## Salida

- **Trámite completado exitosamente:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Incidente duplicado o error amigable:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
