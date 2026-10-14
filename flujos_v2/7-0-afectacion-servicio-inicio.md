# 7.0 - Afectación Servicio - Inicio

Punto de entrada para la verificación en tiempo real de cortes programados, mantenimientos de emergencia o interrupciones en el suministro de agua potable. Deriva hacia la aceptación de la política de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de la opción *Afectación del Servicio* o *Falta de Agua* desde el menú de opciones o derivación por palabras clave vinculadas a desabastecimiento.

## Paso 2. Marcador analítico y configuración de contexto

Al validar la disponibilidad del servicio:
1. Se registra el hito analítico: `Afectación Servicio`.
2. Se configuran las variables de sesión en memoria:
   - `contexto`: `afectacion_servicio`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar el consentimiento de datos personales bajo el contexto `afectacion_servicio`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
