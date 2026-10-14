# 8.0 - Descargar Facturas - Inicio

Punto de entrada para la descarga de facturas y planillas digitales. Establece el contexto de navegación y verifica la autorización de datos personales LOPD antes de proporcionar los enlaces de acceso seguro.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Canal: WhatsApp / Web.

## Paso 1. Configuración de contexto

Al seleccionarse la opción:

1. Registra el hito en analítica: marcador `Descargar Facturas`.
2. Asigna las variables en memoria:
   - `contexto`: `descargar_facturas`
   - `num_intentos_ne`: `0`

## Salida

Transfiere inmediatamente a la validación de políticas legales:

- Transfiere a: [0.1 - LOPD - Inicio](./0-1-lopd-inicio.md).
  *(Al verificar la política, LOPD derivará directamente a [8.1 - Descargar Facturas - Enlace](./8-1-descargar-facturas-enlace.md)).*
