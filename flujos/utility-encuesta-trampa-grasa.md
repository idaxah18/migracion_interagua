# Encuesta de satisfacción - Trampa de grasa

Este flujo recopila la retroalimentación y nivel de satisfacción de los clientes que han completado la solicitud o emisión de su certificado de trampa de grasa. A través de preguntas dinámicas y validadas, evalúa la experiencia general del trámite y almacena los resultados en la base de datos operacional de encuestas de calidad de Interagua.

## Datos que debe traer al entrar

- `{{$memory.nombre_cliente}}`: Nombre completo o de pila del solicitante.
- `{{$user.phone}}`: Número de WhatsApp del cliente para asociar el registro de la encuesta.

## Base de datos de encuestas (Datum)

Los resultados se almacenan y actualizan progresivamente en la tabla de control de calidad:

- **ID de Base de Datos:** `5168`
- **Nombre de tabla / entidad:** `log_encuesta`
- **Campos guardados:**
  - `user_id`: Número de teléfono del usuario (`{{$user.phone}}`).
  - `fecha_hora`: Fecha y hora de captura de la encuesta.
  - `satisfaccion_general`: Calificación del trámite (escala 0 a 10).
  - `facilidad`: Nivel de facilidad del proceso (escala 1 a 5).
  - `comentarios`: Observaciones o sugerencias en texto libre.

## Paso 1. Mensaje de bienvenida a la encuesta

El bot saluda personalmente al usuario agradeciéndole por haber realizado el trámite:

> 👋 Hola {{$memory.nombre_cliente}}
> 
> Queremos agradecerte por haber gestionado tu certificado de trampa de grasa.
> 
> Para nosotros es muy importante escuchar tu opinión y conocer cómo fue tu experiencia con el trámite.

## Paso 2. Pregunta 1 - Satisfacción general (Escala 0 a 10)

El sistema solicita al cliente su valoración global del servicio y almacena la respuesta en `respuesta_1`.

### Validación de la respuesta:
- El valor ingresado debe ser un número entero comprendido entre **0 y 10**.
- **Si el valor está fuera de escala:**
  El bot solicita corregir la entrada:
  > Ups, parece que ese valor no está dentro de la escala 😊. 
  > 
  > Por favor, intentalo nuevamente.
- **Si el valor es válido:**
  Se crea el registro inicial en la base de datos `5168` con la fecha y hora actual y el identificador del usuario.

## Paso 3. Pregunta 2 - Facilidad del trámite (Escala 1 a 5)

El sistema consulta al usuario qué tan fácil le resultó gestionar su trámite a través de la plataforma y almacena el valor en `respuesta_2`.

### Validación de la respuesta:
- El valor ingresado debe ser un número entero entre **1 y 5**.
- **Si el valor está fuera de escala:**
  El bot emite el mensaje de advertencia:
  > Ups, parece que ese valor no está dentro de la escala 😊. 
  > 
  > Por favor, intentalo nuevamente.
- **Si el valor es válido:**
  Se actualiza el registro en la base de datos `5168`, asignando la calificación de facilidad a la fila creada en el paso 1.

## Paso 4. Pregunta 3 - Comentarios y sugerencias

El sistema ofrece un campo abierto para que el cliente detalle libremente sus comentarios, percepciones o áreas de mejora, almacenando el texto en `respuesta_3`.

- La respuesta se actualiza en el campo `comentarios` del registro en la base de datos `5168`.

## Paso 5. Agradecimiento y cierre

Al completar las tres preguntas, el bot emite un mensaje cálido de agradecimiento:

> Agradecemos mucho tu tiempo y tus comentarios 🙌
> 
> ¡Nos ayudan a mejorar para seguir brindándote un mejor servicio! 💧

## Manejo de errores

Si en cualquiera de las escrituras en la base de datos `5168` ocurre una falla de comunicación, la conversación es redirigida a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
