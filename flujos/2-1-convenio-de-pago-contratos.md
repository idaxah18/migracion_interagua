# 2.1 - Convenio de Pago - Contratos

Prepara el entorno técnico y los mensajes interactivos específicos para la identificación y selección del contrato sobre el cual el cliente desea simular o generar un convenio de pago, delegando la captura y validación al flujo central de consulta de contratos.

## Datos que debe traer al entrar

- `contexto`: `convenio_pago`.
- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento de datos (LOPD) verificado positivamente.

## Paso 1. Verificación de horario de mantenimiento

El bot verifica nuevamente el estado operativo del servicio de convenios antes de iniciar la solicitud de credenciales o contratos.

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

Evaluación:
- **En mantenimiento (`enMantenimiento`):** Transfiere de inmediato a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- **Operativo (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Configuración de parámetros y textos dinámicos

El flujo establece en la memoria técnica la URL del servicio SOAP y los textos personalizados que presentará la consulta de contratos:

1. **Configuración de servicio:**
   - `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`

2. **Textos inyectados para la selección de contrato:**
   - `msm_btn_consultaContrato`:
     > ¿Quieres realizar un convenio de pago con tu número de cédula o con tu número de contrato?
   - `msm_btn_listaContratos`:
     > A continuación te presentamos los contratos y direcciones asociados a tu identificación.
     >
     > ¿Sobre cuál quieres realizar tu convenio de pago?

## Salida

- **Continuación normal:** Transfiere a [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) (Skill ID `51072`), el cual presentará las opciones de búsqueda (por contrato o por cédula/RUC) usando los textos configurados.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
