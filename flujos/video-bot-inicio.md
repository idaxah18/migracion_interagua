# Video Bot - Inicio

Este flujo actúa como la puerta de acceso al módulo interactivo de video-asistencia multimedia y educación ciudadana de Interagua. Su propósito es configurar las variables de sesión del usuario antes de desplegar el catálogo de cápsulas de video explicativas sobre consumo responsable, lectura de medidor, prevención de fugas y trámites de agua potable y alcantarillado.

## Datos que debe traer al entrar

- No requiere datos técnicos obligatorios. Se accede desde el menú principal de autoservicio o campañas informativas multimedia.

## Configuración inicial de sesión

Al ingresar al flujo, el sistema prepara la memoria de la conversación:

1. **Establecimiento de contexto:** Asigna la variable `contexto = "video_asistencia"` para recordar en todo momento que el usuario está consultando el catálogo multimedia.
2. **Reinicio de intentos:** Fija la variable `num_intentos_ne = "0"` para evitar que mensajes o selecciones previas interfieran con la interacción actual.
3. **Punto de control (Checkpoint):** Registra el marcador `Video Bot Inicio` para analítica de uso del canal.

## Salida y navegación

- **Validación obligatoria de privacidad y consentimiento:**
  Para dar cumplimiento a la Ley Orgánica de Protección de Datos Personales antes de continuar con la entrega de contenido interactivo, el flujo transfiere al usuario a [0.1 - LOPD - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-1-lopd-inicio.md) (Skill `51066`).
- **Salida de respaldo:**
  En caso de invocación independiente sin parámetros, cuenta con retorno seguro a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
