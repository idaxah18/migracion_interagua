# 5.0 - Fugas de Agua - Inicio

Punto de entrada para el reporte de averías y fugas de agua potable (en veredas, calzadas o guías domiciliarias). Canaliza al usuario hacia la confirmación de la política de protección de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Fugas de Agua* desde el menú de opciones o derivación por palabras clave relacionadas a daños de agua potable.

## Paso 2. Marcador analítico y preparación de sesión

Al validar el servicio operativo:
1. Registra el hito analítico: `Inicio Fugas de Agua`.
2. Establece las variables en memoria:
   - `contexto`: `fuga_agua`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar la autorización de datos personales bajo el contexto `fuga_agua`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
