# Mensaje de error personalizado

Este flujo se encarga de presentar al usuario un mensaje de error explicativo y contextualizado cuando ocurre una regla de negocio no cumplida o una validación específica en algún servicio de Interagua (por ejemplo: cuotas pendientes de convenio previo, solicitudes duplicadas, identificación no autorizada o datos fuera de rango).

En lugar de emitir un fallo genérico del sistema, este componente extrae la explicación clara que generó la lógica previa y se la muestra al cliente antes de devolverlo al inicio para que pueda intentar otra acción.

## Datos que debe traer al entrar

- `{{$memory.msm_error}}`: Texto descriptivo del error o motivo de rechazo de la operación, configurado previamente por el flujo que detectó la novedad.

## Mensaje al cliente

El bot presenta directamente el contenido dinámico almacenado en la variable:

> `{{$memory.msm_error}}`

### Ejemplos comunes de textos que muestra este flujo:
- > "El cliente ya cuenta con un convenio de pago activo en curso."
- > "No es posible registrar el reclamo debido a que ya existe una orden de trabajo abierta para este contrato."
- > "La cuenta seleccionada no presenta valores vencidos para generar una financiación."
- > "El número de identificación no corresponde al titular del contrato."

## Salida

Una vez entregada la notificación de la novedad al usuario, el flujo transfiere la conversación a:
- [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`), reiniciando la sesión para que el cliente pueda consultar otro servicio o formular una nueva consulta.
