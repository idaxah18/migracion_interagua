# 5.0 - Fugas de Agua - Inicio

Punto de entrada para el reporte de averías y fugas de agua potable (en veredas, calzadas o guías domiciliarias). Verifica la disponibilidad operativa de la plataforma de reportes y canaliza al usuario hacia la confirmación de la política de protección de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Fugas de Agua* desde el menú de opciones o derivación por palabras clave relacionadas a daños de agua potable.

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

## Paso 2. Marcador analítico y preparación de sesión

Al validar el servicio operativo:
1. Registra el hito analítico: `Inicio Fugas de Agua`.
2. Establece las variables en memoria:
   - `contexto`: `fuga_agua`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar la autorización de datos personales bajo el contexto `fuga_agua`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
