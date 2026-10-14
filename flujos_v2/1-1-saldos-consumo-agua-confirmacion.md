# 1.1 - Saldos Consumo Agua - Confirmación

Presenta el estado de cuenta detallado del contrato de agua potable seleccionado (saldo adeudado, planillas pendientes, fechas de corte y último pago realizado) y ofrece las opciones para proceder al pago inmediato, solicitar un convenio o realizar otra gestión.

## Datos que debe traer al entrar

- Contrato seleccionado: `{{$memory.Contrato}}` (objeto con `ID_CONTRATO`, dirección, etc.).
- Identificación del cliente: `{{$memory.identificacion}}`.
- Contexto de navegación: `saldos`.

## Paso 1. Consulta de Estado de Cuenta

El flujo inicializa las variables de pagos por defecto:
- `txt_pagos`: "No posee pagos anteriores"
- `fecha_ultimo_pago`: "No posee pagos anteriores"
- `valor_ultimo_pago`: "No posee pagos anteriores"

Servicio: [getEstadoCuenta](../servicios/getEstadoCuenta.md).

Reglas de validación de la respuesta:

1. Si la respuesta trae error, se valida el código:
   - `801543`: El contrato ingresado es incorrecto, o no tiene contratos activos.
   - `20082`: La cédula ingresada es incorrecta.
   - `528`: El contrato ingresado no pertenece al número de identificación registrado en INTERAGUA.
   - Cualquier otro código: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.
2. Si no viene información para el contrato:
   - Se muestra: *No se encontró información para el contrato ingresado*.
3. Si el contrato es válido:
   - Se formatean las fechas de emisión (`FECHA_EMISION`) y vencimiento (`FECHA_VENC_FACT`) en formato `YYYY-MM-DD`.
   - Se evalúa si el usuario tiene deuda (`tieneDeuda` = `DEUDA_PENDIENTE > 0`).
   - Se guarda el objeto `dataEstadoCuenta` en memoria durante 86000 segundos.

## Paso 2. Consulta de Historial de Últimos Pagos

Servicio: [getConsultaPagos](../servicios/getConsultaPagos.md).

Procesamiento de los pagos:

- Si registra pagos anteriores:
  - Toma el primer registro como el pago más reciente.
  - Guarda `valor_ultimo_pago` con `VALOR_PAGADO`.
  - Guarda `fecha_ultimo_pago` con la fecha sin hora.
  - Concatena en `txt_pagos` la lista de los últimos pagos con formato:
    `N - Fecha - $Valor - Banco/Canal`.

## Paso 3. Presentación del Estado de Cuenta

El asistente envía el resumen consolidado al cliente:

> Saldo actual: *${{$memory.dataEstadoCuenta.DEUDA_PENDIENTE}}*  
> Planillas adeudadas: *{{$memory.dataEstadoCuenta.ACCOUNT_WITH_BALANCE}}*  
> Fecha de vencimiento: *{{$memory.dataEstadoCuenta.FECHA_VENC_FACT}}*  
> Fecha de emisión de la última factura: *{{$memory.dataEstadoCuenta.FECHA_EMISION}}*  
> Último pago realizado: $*{{$memory.valor_ultimo_pago}}*  
> Fecha último pago realizado: *{{$memory.fecha_ultimo_pago}}*  

## Paso 4. Opciones de acción

El bot pregunta:

> ¿Qué deseas realizar?

Botones presentados:

1. **Si tiene deuda pendiente (`tieneDeuda` = true):**
   - **Pagar en Línea:** Deriva a la pasarela de recaudación.
   - **Convenio de Pago:** (Si el monto y mora aplican según las políticas comerciales).
   - **Otra Consulta:** Para volver al menú o consultar otro servicio.

2. **Si no tiene deuda (`tieneDeuda` = false):**
   - Muestra que su cuenta está al día.
   - Presenta opciones para ver pagos anteriores o realizar otra consulta.

## Salida

Según la selección del cliente:

- **Pagar en Línea:** Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).
- **Convenio de Pago:** Transfiere a [Convenio de pago](./2-convenio-de-pago.md).
- **Consultar otro contrato de agua:** Transfiere a [_ 1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md).
- **Otra consulta / Salir:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) o [_ 0 - Inicio](./0-inicio.md).
- **En caso de error del servicio:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
