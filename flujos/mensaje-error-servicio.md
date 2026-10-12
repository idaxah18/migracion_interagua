# Mensaje de error de servicio

Este flujo gestiona la respuesta institucional y controlada hacia el cliente cuando se produce una interrupción técnica imprevista, una caída de conectividad, un tiempo de espera agotado (timeout) o un error no controlado en cualquiera de los servidores, APIs externas o bases de datos de Interagua.

Su propósito es garantizar una experiencia de usuario respetuosa y transparente, evitando que el chat quede sin respuesta o exponga códigos técnicos incomprensibles para el ciudadano.

## Datos que debe traer al entrar

- No requiere variables obligatorias de entrada, ya que se invoca de manera transversal desde las ramas de fallo (`failed` o `error`) de cualquier llamada técnica del sistema.

## Mensaje al cliente

El bot presenta un mensaje fijo de disculpa institucional:

> ¡Lo sentimos!, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

## Salida y recuperación

Una vez emitido el mensaje informativo, el flujo deriva la conversación de manera segura a:
- [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`), restableciendo el estado de la conversación para que el usuario pueda intentar otra gestión o reiniciar su interacción sin quedar bloqueado.
