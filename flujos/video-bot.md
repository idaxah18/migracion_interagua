# Video Bot - Guías y cápsulas multimedia

Este flujo entrega a los clientes material audiovisual didáctico, cápsulas explicativas y tutoriales en video para empoderarlos en el control de su servicio de agua potable, la comprensión de su factura y la realización autónoma de lecturas de medidor. Además, orienta sobre la detección temprana de fugas internas y los canales de consulta de saldos.

## Datos que debe traer al entrar

- `contexto`: Identificador del trámite (`video_asistencia`).
- Consentimiento previo de políticas de privacidad completado en [0.1 - LOPD - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-1-lopd-inicio.md).

## Catálogo de módulos audiovisuales

El sistema ofrece una lista interactiva de temáticas con el siguiente encabezado:

> Conoce de una manera fácil todos nuestros servicios, te ayudamos a cuidar tu consumo de agua

Las tres temáticas principales disponibles son:

---

### Módulo 1. Guía de autolectura del medidor

Orientado a clientes que desean verificar personalmente el consumo registrado en su medidor domiciliario antes o durante la facturación:

1. **Introducción:**
   > ¡Hola! Queremos que tengas el control de tu consumo de agua. Por eso, hemos preparado una guía sencilla para que realices tu autolectura de manera fácil y rápida. Acompáñanos en este video y descubre cómo hacerlo en pocos pasos. ¡Es fácil y te ayuda a gestionar mejor tu consumo!
2. **Envío de video y pausa didáctica:**
   El bot remite el video interactivo de autolectura y aplica una pausa técnica de 15 segundos para dar tiempo suficiente a que el cliente descargue y observe el contenido.
3. **Mensaje de cierre y descarga de aplicaciones:**
   Al finalizar el video, el bot presenta opciones para descargar la app móvil de Interagua:
   > ¡Eso es todo! Ahora sabes cómo realizar tu autolectura de manera rápida y sencilla. Si tienes alguna duda, estamos aquí para ayudarte. ¡Gracias por cuidar el agua con nosotros!
   
   Enlaces oficiales a las tiendas de aplicaciones:
   > *Google Play:* https://rebrand.ly/interagua-app  
   > *App Store:* https://rebrand.ly/app-interagua

---

### Módulo 2. Control de incremento de consumo y fugas internas

Destinado a usuarios que detectan un valor inusual en su consumo y requieren pautas claras sobre cómo inspeccionar su predio:

1. **Información preventiva:**
   > Si notas un incremento en tu consumo de agua potable, primero debes revisar que no existan desperfectos en los puntos de agua dentro de tu domicilio. 
   > 
   > Es responsabilidad del usuario mantener las instalaciones en buen estado de funcionamiento desde el medidor hasta el interior del hogar.
2. **Cápsula en video:**
   Se remite el video instructivo demostrando cómo cerrar llaves de paso, verificar el movimiento de las manecillas del medidor y detectar fugas invisibles en inodoros y cisternas.

---

### Módulo 3. Canales de consulta de saldo

Enseña al ciudadano todas las opciones digitales y físicas disponibles para revisar su saldo pendiente:

1. **Mensaje explicativo:**
   > ¿Necesitas consultar tu saldo? Aquí te explicaremos los diferentes canales de atención que tienes disponibles para hacerlo. Simplemente selecciona la opción a continuación y te mostraremos cómo acceder a esta información fácilmente. ¡Vamos a empezar!
2. **Video de canales de consulta:**
   Entrega la cápsula audiovisual y despliega una lista interactiva de opciones complementarias:
   > ¡Espero que esta información te haya sido útil para consultar tu saldo! Si tienes alguna otra pregunta o necesitas ayuda con otro servicio, no dudes en seleccionarlo a continuación. Estamos aquí para ayudarte.

---

## Asistencia humana y salidas del flujo

- **Solicitud de asesor:** Si en cualquier momento el usuario requiere soporte especializado, el sistema intenta transferirlo a un agente (`JelouConnect`):
  - Si no hay agentes disponibles: deriva a [Operator Not Found](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-not-found.md) (Skill `51040`).
  - Si está fuera de horario de atención: deriva a [Operator Out Schedule](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-out-schedule.md) (Skill `51041`).
- **Retorno al menú:** El cliente puede optar por regresar en cualquier momento a [0.3 - Menú Principal - Bienvenida](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-3-menu-principal-bienvenida.md) (Skill `51161`) o a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
