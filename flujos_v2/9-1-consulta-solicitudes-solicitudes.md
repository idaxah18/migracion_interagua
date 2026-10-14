# 9.1 - Consulta Solicitudes - Solicitudes

Permite al cliente rastrear el estado de cualquier trámite registrado ante Interagua mediante su número de contrato o su número de solicitud. Muestra la fecha de radicación, tipología, causal y estado de avance, y ofrece la opción de despachar el documento resolutivo oficial en formato PDF al correo electrónico registrado.

## Datos que debe traer al entrar

- `contexto`: `consulta_solicitudes`.
- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento de datos (LOPD) verificado positivamente.

## Paso 1. Selección del método de búsqueda

El bot inicializa la URL de consulta:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`

Y presenta los botones de selección:

> ¿Conoces tu número de solicitud o tu número de contrato?

Opciones con botones:
1. **Contrato:** Solicita el contrato (Paso 2A).
2. **Número de solicitud:** Solicita el identificador del trámite (Paso 2B).

## Paso 2A. Búsqueda por número de contrato

El bot solicita el número de contrato:

> Por favor ingresa tu número de contrato.

Servicio: [getSolicitudPorContrato](../servicios/getSolicitudPorContrato.md).

Mapeo de errores:
- `902000`: `No se encontró respuesta configurada para la solicitud ingresada.`
- `3303`, `801543`: `El contrato ingresado es incorrecto, o no tiene contratos activos.`
- Sin solicitudes devueltas: `No se encontraron solicitudes para el contrato consultado.`
- Fallback: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si hay múltiples solicitudes:
- Se presenta el menú numerado de expedientes (`menu_solicitudes`):
  > {{$context.menu_solicitudes}}
- El usuario escribe el número de la solicitud que desea inspeccionar o escribe *Salir*.
- Al elegir una, se recupera el detalle de la solicitud seleccionada y continúa al Paso 3.

## Paso 2B. Búsqueda por número de solicitud

El bot solicita el código del expediente:

> Por favor ingresa tu número de solicitud.

Servicio: [getSolicitud](../servicios/getSolicitud.md).

Mapeo de errores:
- `902000`: `No se encontró respuesta configurada para la solicitud ingresada.`
- `3303`, `801543`: `El contrato ingresado es incorrecto, o no tiene contratos activos.`
- Sin datos devueltos: `No se encontraron datos para la solicitud consultada.`
- Fallback: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si la búsqueda es exitosa, continúa al Paso 3.

## Paso 3. Presentación del estado del trámite

El bot despliega la ficha informativa de la solicitud:

> ¡Perfecto! A continuación te presento los datos de tu solicitud:  
>   
> ▪ No. solicitud: {{$memory.Solicitud.PACKAGE_ID}}  
> ▪ Fecha: {{$memory.Solicitud.F_REGISTRO}}  
> ▪ Descripción: {{$memory.Solicitud.DESC_TIPO_SOLI}}  
> ▪ Actividad: {{$memory.Solicitud.DESC_CAUSAL_ACTIVIDAD}}  
> ▪ Estado: {{$memory.Solicitud.DESC_EST_TRAMITE}}

Opciones con botones:
1. **Enviar resolución por correo:** Inicia el procedimiento del Paso 4.
2. **Finalizar / Salir:** Transfiere a [_ Atención Adicional](./atencion-adicional.md).

## Paso 4. Envío de resolución PDF por correo electrónico

1. **Consulta de correos registrados:**  
   Servicios: [getDatosCliente](../servicios/getDatosCliente.md) y [getListadoCorreoCliente](../servicios/getListadoCorreoCliente.md).  
   - Si no existen correos vinculados:  
     > No se encontró un correo electrónico asociado a tu contrato.  
     *(Transfiere a soporte manual).*
   - Si existen correos: Presenta la lista numerada para que el usuario elija a cuál desea recibir el PDF.

2. **Despacho del documento:**  
   Servicio: [solicitudReclamos](../servicios/solicitudReclamos.md)  
   - Si el envío tiene éxito:  
     > Se ha enviado el PDF a tu correo seleccionado
   - Si el envío falla:  
     > Ha ocurrido un error al enviar el PDF a tu correo

## Salida

- **Culminación del trámite:** Transfiere a [_ Atención Adicional](./atencion-adicional.md).
- **Asistencia manual:** Transfiere a [_ Ayuda Personalizada](./ayuda-personalizada.md).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
