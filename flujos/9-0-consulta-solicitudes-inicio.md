# 9.0 - Consulta Solicitudes - Inicio

Punto de entrada para el seguimiento y verificación del estado de solicitudes, quejas y reclamos ingresados en Interagua. Configura el contexto operativo y transfiere a la verificación de la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Consulta de Solicitudes* o *Estado de Trámite* desde el Menú Principal o palabras clave relacionadas.

## Paso 1. Marcador analítico y configuración de contexto

Al iniciar el flujo:
1. Registra el hito analítico: `Consulta Solicitudes`.
2. Establece las variables en la memoria de la conversación:
   - `contexto`: `consulta_solicitudes`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto `consulta_solicitudes`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
