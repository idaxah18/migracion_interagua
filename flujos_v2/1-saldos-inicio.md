# 1 - Saldos - Inicio

Punto de entrada cuando el usuario selecciona la opción "Consulta de Saldos" desde el Menú Principal.

Establece el contexto de negocio `saldos` y valida si el cliente cuenta con autorización LOPD activa antes de consultar las deudas de su cuenta.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Canal: WhatsApp / Web.

## Paso 1. Configuración de contexto

Al activarse la opción:

1. Registra el hito en analítica: marcador `Saldos Menu`.
2. Asigna la variable en memoria:
   - `contexto`: `saldos`

## Salida

Transfiere inmediatamente al flujo de verificación de consentimiento legal:

- Transfiere a: [0.1 - LOPD - Inicio](./0-1-lopd-inicio.md).
  *(Una vez verificada la política, LOPD devolverá al usuario automáticamente a [1 - Saldos - Menu](./1-saldos-menu.md)).*
