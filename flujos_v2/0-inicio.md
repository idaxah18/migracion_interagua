# Inicio

Punto de entrada principal para cualquier usuario que inicia una conversación con el asistente virtual de Interagua en Web.

Establece el contexto inicial de navegación y deriva inmediatamente al flujo de validación de protección de datos personales antes de permitir cualquier consulta.

## Datos que debe traer al entrar

- Número de teléfono del usuario (identificador de canal en WhatsApp: `{{phone}}`).
- Nombre del perfil registrado del usuario (`{{names}}`).
- Canal de comunicación: Web, Facebook o Instagram.

## Paso 1. Establecimiento de contexto

Al recibir el primer mensaje o saludo del usuario, el flujo ejecuta:

1. Registra el hito en analítica: marcador `Inicio`.
2. Asigna la variable en memoria:
   - `contexto`: `menu_principal`

## Salida

El flujo finaliza derivando de inmediato al usuario al flujo de validación de políticas:

- Transfiere a: [0.1 - LOPD - Inicio](./0-1-lopd-inicio.md).
