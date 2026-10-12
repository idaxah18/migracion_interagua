# Reclamo Técnico - Quejas

Formaliza quejas e inconformidades por deficiencias en la prestación del servicio técnico de agua y alcantarillado. Solicita observaciones del cliente, aplica limpieza ortográfica para evitar errores de codificación en el backend, invoca el servicio `Register Queja`, y entrega la constancia de radicación.

## Datos que debe traer al entrar

- `codigoProducto`, `codigoCliente`, `codigoDireccion`, `codigoDireccionProducto`: Parámetros técnicos del contrato y del predio.
- `codigoCausal`: Código técnico asignado al motivo de la queja.
- `codigoTipoPaquete`: Tipo de paquete asignado en la tipificación.
- `tituloServicio`: Descripción amigable de la queja.
- `numeroContrato`: Código de cuenta o contrato.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `user.phone`: Teléfono del usuario.

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
1. Se depuran tildes y diacríticos.
2. Se eliminan caracteres especiales dejando exclusivamente texto y espacios.
3. Se prefija el canal: `WH <telefonoUsuario> <comentario_limpio>`.

## Paso 3. Registro formal de la queja

Servicio: **Register Queja** (Tool ID `2952`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "url": "{{$memory.urlService}}",
  "comentario": "<comentario>",
  "codigoCausal": "<codigoCausal>",
  "codigoCliente": "<codigoCliente>",
  "identificacion": "<identificacion>",
  "codigoDireccion": "<codigoDireccion>",
  "codigoTipoPaquete": "<codigoTipoPaquete>",
  "informacionContrato": "<numeroContrato>",
  "codigoDireccionProducto": "<codigoDireccionProducto>"
}
```

Evaluación de la respuesta:
1. **Queja en curso preexistente:**  
   - Código `903034`: `Actualmente ya cuentas con una solicitud en estado registrado por el mismo concepto.`  
   Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`).
2. **Sin paquete devuelto:**  
   Mensaje: `Estimado cliente no hemos podido registrar tu queja.`
3. **Falla de conexión o error general:**  
   Mensaje: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`  
   Deriva a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).

Si es exitoso:
- Se obtiene el código `PACKAGE_ID` en `numeroSolicitud`.

## Paso 4. Entrega de confirmación oficial

El bot envía el mensaje formal de ingreso:

> Tu número de Solicitud {{$memory.numeroSolicitud}} por {{$memory.tituloServicio}} ha sido ingresado exitosamente 👍

## Salida

- **Trámite completado exitosamente:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Queja duplicada o mensaje amigable:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
