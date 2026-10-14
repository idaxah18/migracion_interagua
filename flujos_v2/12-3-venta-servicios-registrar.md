# 12.3 - Venta Servicios - Registrar

Formaliza la radicación de solicitudes de venta de servicios de ingeniería (como la limpieza o evaluación de pozos sépticos). captura observaciones opcionales del cliente, invoca el servicio `Get Solicitud Venta Servicios Ingenieria`, consulta los detalles del trámite generado y ofrece el enlace al agendamiento de citas o pago de valores.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato.
- `identificacion`: Identificación del solicitante.
- `datos_cliente`: Datos del beneficiario (`NOMBRE_BENEFICIARIO`, `DIRECCIONTRADICIONAL`, `IDENTIFICACION_BENEF`).
- `correo_selected`: Correo electrónico del cliente.
- `actividadPozoSeptico`: Actividad seleccionada.
- `urlService`: URL del servicio SOAP.
- `user.phone`: Teléfono móvil del cliente.

## Paso 2. Captura opcional de observaciones

El bot pregunta con botones:

> ¡Gracias! ¿Te gustaría agregar alguna observación para tu solicitud?

Opciones con botones:
1. **Sí:** Solicita el texto libre y formatea: `comentario = "WH " + telefonoUsuario + " - " + observacion.slice(0, 150)`.
2. **No:** Establece `comentario = ""` y procede directamente al registro.

## Paso 3. Creación formal de la solicitud de ingeniería

Servicio: [getSolicitudVentaServiciosIngenieria](../servicios/getSolicitudVentaServiciosIngenieria.md).

Si el servicio genera la solicitud exitosamente:
- Se extrae `packageId` desde `data.PACKAGE_ID`.

## Paso 4. Verificación del trámite y programación

Servicio: [getSolicitud](../servicios/getSolicitud.md).

Calcula la fecha tentativa de inspección (`format_fecha` = día siguiente, `dias = 4`) y transfiere al flujo de agendamiento [_ 14 Agendamiento Citas](./14-agendamiento-citas.md) para coordinar la visita técnica.

En caso de que el trámite requiera cancelación previa de valores (código `20201`):
- Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md) para que el usuario abone el costo por pasarela digital.

## Salida

- **Trámite generado:** Transfiere a [_ 14 Agendamiento Citas](./14-agendamiento-citas.md) o [_ Atención Adicional](./atencion-adicional.md).
- **Pendiente de pago previo:** Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
