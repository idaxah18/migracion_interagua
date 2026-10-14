# 11.0 - Faltante de Tapa - Inicio

Punto de entrada directo para reportar el peligro y daño ocasionado por la ausencia de tapas de alcantarillado en aceras o calzadas (bocas de registro o cajas domiciliarias). Deriva hacia la aceptación de la política de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección directa de *Faltante de Tapa* o derivación por palabras clave vinculadas a tapas de alcantarilla sustraídas o rotas.

## Paso 2. Marcador analítico y configuración de contexto

Al validar la disponibilidad del servicio:
1. Registra el hito analítico: `Faltante de tapa Ini`.
2. Establece las variables en memoria:
   - `contexto`: `faltante_tapa`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar la autorización de datos personales bajo el contexto `faltante_tapa`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
