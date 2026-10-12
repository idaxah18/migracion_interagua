# Asesores fuera de horario (Operator Out Schedule)

Este flujo se activa cuando un usuario solicita conectarse con un asesor humano en vivo o el sistema intenta transferirlo, pero el requerimiento se produce fuera del horario laboral establecido para la atención humana de Interagua (evento `OPERATORS_NOT_IN_SCHEDULER`).

En esta eventualidad, el sistema ofrece una alternativa guiada de autoservicio: despliega el catálogo de servicios disponibles en línea para que el cliente pueda autogestionar su requerimiento sin necesidad de aguardar la apertura del centro de atención.

## Datos que debe traer al entrar

- `servicios_disponibles`: Arreglo en memoria con el catálogo de servicios técnicos y comerciales vigentes.
- Horario de atención humana de los operadores de Interagua.

## Paso 1. Presentación del menú interactivo de servicios

El sistema formatea la lista de servicios activos y le presenta al usuario un menú enumerado en texto:

> Estos son nuestros servicios:
> 
> 1 - {DESCRIPCION_SERVICIO_1}  
> 2 - {DESCRIPCION_SERVICIO_2}  
> ...  
> 
> *Ingresa 0 para salir del menú*

### Casos especiales en la generación del menú:
- **Sin servicios disponibles:** Si la lista se encuentra vacía o el servicio de catálogos falla, el bot comunica:
  > No existen servicios disponibles por el momento
- **Salir del menú:** Si el usuario digita `0`, el bot responde:
  > Has salido del menú.
  Y finaliza la interacción de forma limpia.

## Paso 2. Validación de la selección

El usuario ingresa el número correspondiente al servicio que desea consultar:

- **Si ingresa un número fuera de rango:**
  El bot emite una alerta de corrección y vuelve a solicitar la opción:
  > La opción que ingresaste no es correcta. Por favor. inténtalo nuevamente.

- **Si la opción es válida:**
  El sistema recupera la ficha del servicio seleccionado y evalúa su tipo:
  - **Certificado o inspección de trampa de grasa (`PE_TIPO_VTA_INGE_00`):**
    El bot muestra la descripción detallada del procedimiento y requisitos:
    > `{{$memory.servicio_selected.EXPLICACION_BREVE}}`
  - **Cualquier otro servicio general:**
    El bot confirma la recepción de la solicitud:
    > Gracias por tu selección.

## Salidas del flujo

- **Salida regular:** Tras mostrar la información del servicio o confirmar la selección, el cliente puede continuar interactuando con el bot o cerrar la conversación.
- **Invocación independiente:** Si el flujo es llamado directamente desde el enrutador inicial, deriva de forma segura a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
