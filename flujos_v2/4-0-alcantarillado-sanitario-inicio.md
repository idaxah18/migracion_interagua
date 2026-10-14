# 4.0 - Alcantarillado Sanitario - Inicio

Punto de entrada para el reporte de reboses, obstrucciones o problemas en las redes de alcantarillado sanitario (aguas servidas). Orienta al usuario hacia la aceptación de la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Alcantarillado Sanitario* desde el menú de opciones o derivación por palabras clave vinculadas a aguas servidas.

## Paso 2. Marcador analítico y preparación de sesión

Al validar el servicio operativo:
1. Registra el hito analítico: `Alcantarillado Inicio`.
2. Establece las variables en memoria:
   - `contexto`: `alcantarillado_sanitario`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar la autorización de datos personales bajo el contexto `alcantarillado_sanitario`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
