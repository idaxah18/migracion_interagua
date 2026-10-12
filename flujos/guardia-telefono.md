# Guardia de teléfono

Este componente actúa como una compuerta de seguridad previa a la ejecución de trámites que requieren contactabilidad telefónica directa. Su objetivo es comprobar que la sesión cuente con el número de teléfono móvil del usuario en WhatsApp. Si el usuario ya cuenta con su teléfono registrado, le permite continuar de inmediato sin interrupciones; si el número no está disponible, le solicita compartirlo utilizando la función nativa de WhatsApp.

## Datos que debe traer al entrar

- `{{$user.phone}}`: Número de teléfono del usuario registrado en el canal de mensajería.
- Contexto o flujo de origen que requiere validar la identidad del número.

## Paso 1. Comprobación del número de teléfono

El flujo revisa de forma automática si la variable `user.phone` se encuentra vacía:

- **Si el teléfono ya está presente:** El flujo no interrumpe al usuario ni envía ningún mensaje. Termina con estado de **Éxito** (`UfXBONZIvelRkXr-3R5vr`) y permite que el trámite continúe con normalidad.
- **Si el teléfono está vacío:** Se activa el mecanismo interactivo de solicitud de contacto.

## Paso 2. Solicitud interactiva de contacto

Cuando no se dispone del número telefónico, se envía al cliente una tarjeta de solicitud nativa de contacto de WhatsApp con el siguiente mensaje:

> Para continuar necesitamos confirmar tu número de contacto. Usa el botón para compartirlo.

### Reglas de validación y comportamiento del mensaje:
- **Bloqueo interactivo:** La conversación espera a que el usuario presione el botón nativo de WhatsApp para compartir su tarjeta de contacto.
- **Mensaje de reintento si escribe texto:** Si el usuario intenta escribir texto libre en lugar de presionar el botón, el bot le recuerda:
  > Por favor, comparte tu contacto con el botón.
- **Sin teléfonos alternos vacíos:** No se admiten contactos que no incluyan un número válido.

## Salidas del flujo

- **Contacto compartido exitosamente:** Al recibir el contacto con su número, el flujo culmina con resultado de **Éxito** (`UfXBONZIvelRkXr-3R5vr`), retornando al flujo principal que requería el dato.
- **Expiración de tiempo:** Si el usuario no comparte su contacto dentro de la ventana de espera, el flujo culmina con el estado **Sin teléfono** (`M3U-CGLhrD1agVsViSwUC`).
- **Invocación independiente:** Si el flujo es llamado directamente como punto de entrada sin subflujo receptor, deriva a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
