# Reclamos - Inicio

Punto de entrada general para la radicación y gestión de reclamos técnicos y comerciales. Valida el estado operativo de los servicios de reclamos de Interagua y deriva al usuario hacia la aceptación o verificación de la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de la opción *Reclamos* desde el Menú Principal o derivación por palabras clave vinculadas a quejas e inconvenientes.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- **En mantenimiento (`enMantenimiento`):** Transfiere de inmediato a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- **Operativo (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Marcador analítico y configuración de contexto

Al validar la disponibilidad del servicio:
1. Se registra el hito analítico: `Reclamos`.
2. Se configuran las variables en la memoria de la conversación:
   - `contexto`: `reclamos_general`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto `reclamos_general`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
