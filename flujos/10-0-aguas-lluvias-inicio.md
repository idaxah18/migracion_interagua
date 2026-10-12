# 10.0 - Aguas Lluvias - Inicio

Punto de entrada para reportes técnicos relacionados con el sistema de drenaje pluvial (sumideros obstruidos, inundaciones en calzadas o rejillas faltantes). Valida el estado operativo del servicio y canaliza al usuario hacia la confirmación de la política de protección de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Aguas Lluvias* desde el menú de reclamos o derivación por palabras clave vinculadas a drenaje pluvial.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- **En mantenimiento (`enMantenimiento`):** Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- **Operativo (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Marcador analítico y configuración de contexto

Al validar la disponibilidad del servicio:
1. Registra el hito analítico: `Aguas lluvias inicio`.
2. Establece las variables en memoria:
   - `contexto`: `aguas_lluvia`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar la autorización de datos personales bajo el contexto `aguas_lluvia`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
