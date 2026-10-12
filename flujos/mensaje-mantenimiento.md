# Mensaje de mantenimiento

Este flujo gestiona la atención del usuario cuando un servicio de Interagua se encuentra temporalmente inhabilitado por una ventana de mantenimiento programado en sus sistemas centrales. En lugar de interrumpir la atención de forma abrupta, el bot informa con calidez sobre la indisponibilidad temporal y ofrece la posibilidad de transferir el caso a un asesor humano para que reciba asistencia personalizada.

## Datos que debe traer al entrar

- `fechaInicio`: Fecha y hora de inicio de la ventana de mantenimiento (formato `DD/MM/YYYY HH:mm:ss`).
- `fechaFin`: Fecha y hora de culminación del mantenimiento (formato `DD/MM/YYYY HH:mm:ss`).

## Servicio: Horario de Mantenimiento

Antes o durante la gestión del incidente, el sistema evalúa si la hora actual de Ecuador (UTC-5) se encuentra dentro de la ventana de mantenimiento programada.

- **Nombre de la herramienta:** Horario de Mantenimiento
- **ID de la herramienta:** `6070`
- **Método / Ejecución:** Función de cálculo interno de fecha y hora local
- **Parámetros de entrada:**
  ```json
  {
    "fechaInicio": "11/03/2025 15:00:00",
    "fechaFin": "11/04/2025 10:00:00"
  }
  ```
- **Salidas:**
  - `enMantenimiento`: La fecha actual está comprendida entre el inicio y el fin del mantenimiento.
  - `fueraDeMantenimiento`: La fecha actual está fuera del rango del mantenimiento.

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
  Se transfiere a [Operator Not Found](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-not-found.md) (Skill `51040`).

- **Si se encuentra fuera del horario de atención de los asesores (`OPERATORS_NOT_IN_SCHEDULER`):**
  Se transfiere a [Operator Out Schedule](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-out-schedule.md) (Skill `51041`).

## Salida alternativa

Si el usuario selecciona **No** en la pregunta inicial:
- Se transfiere al flujo de [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`), donde se le pregunta si desea realizar otra gestión antes de despedirse.
