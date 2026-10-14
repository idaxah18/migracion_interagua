# Asesores fuera de horario

Se usa cuando el cliente pide un asesor fuera del horario de atención humana. En ese caso se muestra el catálogo de servicios que puede gestionar solo.

## Datos de entrada

- `servicios_disponibles`: catálogo vigente de servicios.
- Horario de atención de los asesores.

## 1. Menú de servicios

> Estos son nuestros servicios:
>
> 1 - {DESCRIPCION_SERVICIO_1}
> 2 - {DESCRIPCION_SERVICIO_2}
> ...
>
> *Ingresa 0 para salir del menú*

- Si no hay servicios: *No existen servicios disponibles por el momento*.
- Si escribe `0`: *Has salido del menú.* y termina.

## 2. Selección

- Número fuera de rango: *La opción que ingresaste no es correcta. Por favor. inténtalo nuevamente.*
- Trampa de grasa (`PE_TIPO_VTA_INGE_00`): se muestra `{servicio_selected.EXPLICACION_BREVE}`.
- Cualquier otro servicio: *Gracias por tu selección.*

## Salida

Después de la selección el cliente sigue en la conversación o la cierra. Si el flujo se abre sin catálogo, vuelve a [0 - Inicio](./0-inicio.md).
