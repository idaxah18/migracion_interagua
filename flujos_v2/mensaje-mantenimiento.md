# Mensaje de mantenimiento

Este flujo gestiona la atención del usuario cuando un servicio de Interagua se encuentra temporalmente inhabilitado por una ventana de mantenimiento programado en sus sistemas centrales. En lugar de interrumpir la atención de forma abrupta, el bot informa con calidez sobre la indisponibilidad temporal y ofrece la posibilidad de transferir el caso a un asesor humano para que reciba asistencia personalizada.

## Paso 1. Notificación y ofrecimiento de asesoría

El bot presenta al usuario un mensaje interactivo con dos opciones en botones:

> ¡Lo sentimos!, el servicio no está disponible actualmente.
> 
> ¿Deseas que un asesor se comunique contigo para revisar esta novedad?

Opciones disponibles:
1. **Sí** (Desea hablar con un asesor)
2. **No** (Prefiere no esperar asesoría)

## Paso 2. Recopilación de datos para el asesor

Si el cliente responde que **Sí**:

1. **Solicitud de nombre:**
   El bot consulta el nombre completo del solicitante y lo guarda en la variable `nombres_asesoria`.
2. **Detalle de la novedad:**
   A continuación, solicita que describa el motivo de su consulta o requerimiento y lo almacena en `requerimiento_asesoria`.

## Paso 3. Conexión con un asesor humano

El sistema intenta asignar la conversación a un operador disponible mediante el nodo de conexión inteligente (`JelouConnect`):

- **Si la transferencia es exitosa:**
  El bot emite el mensaje de confirmación:
  > Por favor permíteme validar la información que nos proporcionaste 🙂
  Y la conversación pasa al panel del agente humano asignado.

- **Si no hay operadores disponibles (`OPERATOR_NOT_FOUND` o `UNKNOWN_ERROR`):**
  Se transfiere a [Operador no disponible](./operator-not-found.md).

- **Si se encuentra fuera del horario de atención de los asesores (`OPERATORS_NOT_IN_SCHEDULER`):**
  Se transfiere a [Asesores fuera de horario](./operator-out-schedule.md).

## Salida alternativa

Si el usuario selecciona **No** en la pregunta inicial:
- Se transfiere al flujo de [Atención Adicional](./atencion-adicional.md), donde se le pregunta si desea realizar otra gestión antes de despedirse.
