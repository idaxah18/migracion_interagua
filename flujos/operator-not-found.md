# Operador no disponible (Operator Not Found)

Este flujo se activa cuando un usuario solicita ser transferido a un agente de atención humana o cuando el sistema intenta derivarlo ante una dificultad, pero todos los operadores de la plataforma se encuentran ocupados o no hay asesores conectados en la cola de atención en ese instante.

Su objetivo es brindarle tranquilidad al usuario, asegurarle que su requerimiento quedó registrado en la cola de espera y permitirle retomar la navegación del bot si así lo desea.

## Datos que debe traer al entrar

- No requiere datos obligatorios previos. Se invoca automáticamente ante la respuesta `OPERATOR_NOT_FOUND` o `UNKNOWN_ERROR` procedente de un intento de conexión con operadores (`JelouConnect`).

## Mensaje al cliente

El bot presenta un mensaje claro e informativo:

> En este momento no hay asesores disponibles. En breve un asesor se contactará contigo.

## Salida

Tras entregar el mensaje de aviso al usuario:
- El flujo deriva a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`), dejando la sesión lista para continuar o permitiendo que el cliente realice consultas adicionales mientras espera el contacto de un asesor.
