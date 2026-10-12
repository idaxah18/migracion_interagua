# 2.0 - Convenio de Pago - Inicio

Punto de entrada al trámite de convenio de pago y financiamiento de planillas vencidas. Valida si la plataforma de convenios se encuentra en ventana operativa o en mantenimiento técnico programado antes de proceder a la verificación de políticas de datos y consulta de contratos.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de la opción *Convenio de Pago* desde el Menú Principal o derivación directa por palabra clave.

## Paso 1. Verificación de horario de mantenimiento

El bot consulta el servicio de control operativo para verificar si el sistema de convenios de pago se encuentra en mantenimiento programado.

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

Evaluación de la respuesta:
1. **En horario de mantenimiento (`enMantenimiento`):** El sistema suspende temporalmente la atención de convenios y transfiere de inmediato a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
2. **Operación normal (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Marcador analítico y configuración de contexto

Al superar la validación técnica:
1. Se registra el hito en analítica: `Convenio Pago`.
2. Se configuran las variables en la memoria de la conversación:
   - `contexto`: `convenio_pago`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación del trámite:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto de convenio de pago.
- **En caso de error o reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
