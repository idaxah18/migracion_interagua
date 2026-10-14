# Reclamos - Inicio

Punto de entrada general para la radicación y gestión de reclamos técnicos y comerciales. Deriva al usuario hacia la aceptación o verificación de la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de la opción *Reclamos* desde el Menú Principal o derivación por palabras clave vinculadas a quejas e inconvenientes.

## Paso 2. Marcador analítico y configuración de contexto

Al validar la disponibilidad del servicio:
1. Se registra el hito analítico: `Reclamos`.
2. Se configuran las variables en la memoria de la conversación:
   - `contexto`: `reclamos_general`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar el consentimiento de datos personales bajo el contexto `reclamos_general`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
