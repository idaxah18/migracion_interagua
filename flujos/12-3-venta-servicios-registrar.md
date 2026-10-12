# 12.3 - Venta Servicios - Registrar

Formaliza la radicación de solicitudes de venta de servicios de ingeniería (como la limpieza o evaluación de pozos sépticos). Valida la disponibilidad de teléfono móvil, captura observaciones opcionales del cliente, invoca el servicio `Get Solicitud Venta Servicios Ingenieria`, consulta los detalles del trámite generado y ofrece el enlace al agendamiento de citas o pago de valores.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato.
- `identificacion`: Identificación del solicitante.
- `datos_cliente`: Datos del beneficiario (`NOMBRE_BENEFICIARIO`, `DIRECCIONTRADICIONAL`, `IDENTIFICACION_BENEF`).
- `correo_selected`: Correo electrónico del cliente.
- `actividadPozoSeptico`: Actividad seleccionada.
- `urlService`: URL del servicio SOAP.
- `user.phone`: Teléfono móvil del cliente.

## Paso 1. Verificación de teléfono de contacto

El sistema valida el número celular:
- **Sin teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`). Si no se recibe:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
- **Con teléfono:** Continúa al Paso 2.

## Paso 2. Captura opcional de observaciones

El bot pregunta con botones:

> ¡Gracias! ¿Te gustaría agregar alguna observación para tu solicitud?

Opciones con botones:
1. **Sí:** Solicita el texto libre y formatea: `comentario = "WH " + telefonoUsuario + " - " + observacion.slice(0, 150)`.
2. **No:** Establece `comentario = ""` y procede directamente al registro.

## Paso 3. Creación formal de la solicitud de ingeniería

Servicio: **Get Solicitud Venta Servicios Ingenieria** (Tool ID `6439`)  
Endpoint: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "contrato": "<numeroContrato>",
  "actividad": "<actividadPozoSeptico>",
  "comentario": "<comentario>",
  "comentario2": "PE_VTA_INGE_DESC_32",
  "comentario3": "112.33",
  "identificacion": "<identificacion>",
  "telefonoContacto": "<telefono_contacto>",
  "correoElectronico": "<correo_selected>",
  "nombreBeneficiario": "<NOMBRE_BENEFICIARIO>",
  "direccionBeneficiario": "<DIRECCIONTRADICIONAL>",
  "identificacionBeneficiario": "<IDENTIFICACION_BENEF>"
}
```

Si el servicio genera la solicitud exitosamente:
- Se extrae `packageId` desde `data.PACKAGE_ID`.

## Paso 4. Verificación del trámite y programación

Servicio: **Get Solicitud** (Tool ID `3036`)  
Request:
```json
{
  "env": { "value": "Produccion" },
  "identificacion": "<identificacion>",
  "numeroSolicitud": "<packageId>"
}
```

Calcula la fecha tentativa de inspección (`format_fecha` = día siguiente, `dias = 4`) y transfiere al flujo de agendamiento [_ 14 Agendamiento Citas](./14-agendamiento-citas.md) (Skill ID `51100`) para coordinar la visita técnica.

En caso de que el trámite requiera cancelación previa de valores (código `20201`):
- Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md) (Skill ID `51053`) para que el usuario abone el costo por pasarela digital.

## Salida

- **Trámite generado:** Transfiere a [_ 14 Agendamiento Citas](./14-agendamiento-citas.md) (Skill ID `51100`) o [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Pendiente de pago previo:** Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md) (Skill ID `51053`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
