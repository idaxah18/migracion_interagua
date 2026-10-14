# 1.2 - Saldos TRB - Confirmación

Muestra el estado de cuenta y planillas adeudadas de la Tasa de Recolección de Basura (TRB) para el contrato seleccionado, junto con el último pago registrado en el sistema.

## Datos que debe traer al entrar

- Contrato seleccionado: `{{$memory.Contrato}}`.
- Identificación: `{{$memory.identificacion}}`.
- Contexto: `saldo_trb`.

## Paso 1. Consulta de Estado de Cuenta TRB

Servicio: [getEstadoCuentaTRB](../servicios/getEstadoCuentaTRB.md).

Reglas de respuesta del servicio:

1. Si responde con error, se usa el código:
   - `801543`: El contrato ingresado es incorrecto, o no tiene contratos activos.
   - `20082`: La cédula ingresada es incorrecta.
   - `528`: El contrato ingresado no pertenece al número de identificación registrado en INTERAGUA.
   - Cualquier otro código: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.
2. Si no viene información:
   - *No se encontró información para el contrato ingresado.*
3. Si el contrato es válido:
   - Se formatean las fechas de emisión y vencimiento (eliminando la parte horaria).
   - Se calcula `tieneDeuda` = `parseFloat(data.DEUDA_PENDIENTE) > 0`.
   - Se guarda `dataEstadoCuentaTRB` en memoria durante 86400 segundos.

## Paso 2. Consulta de Pagos TRB

Servicio: [getConsultaPagosTRB](../servicios/getConsultaPagosTRB.md).

Procesamiento:
- Si existen pagos anteriores, toma el primer registro y extrae `valor_ultimo_pago` y `fecha_ultimo_pago`.
- Genera la lista formateada `txt_pagos` con el histórico de pagos de recolección de basura.

## Paso 3. Presentación del saldo TRB

El bot muestra el detalle en pantalla:

> A continuación te presento el saldo de la *TRB* - “Tasa de Recolección de Basura”:  
> 
> *Saldo actual:* ${{$memory.dataEstadoCuentaTRB.DEUDA_PENDIENTE}}  
> *Planillas adeudas:* {{$memory.dataEstadoCuentaTRB.ACCOUNT_WITH_BALANCE}}  
> *Fecha de emisión de la última factura:* {{$memory.dataEstadoCuentaTRB.FECHA_EMISION}}  
> *Fecha de vencimiento de la última factura:* {{$memory.dataEstadoCuentaTRB.FECHA_VENC_FACT}}  
> Último pago realizado: $*{{$memory.valor_ultimo_pago}}*  
> Fecha último pago realizado: *{{$memory.fecha_ultimo_pago}}*  

## Paso 4. Opciones de continuidad

El asistente pregunta:

> ¿Qué deseas realizar?

Botones presentados:

1. **Si tiene deuda:**
   - **Pagar Factura:** Para redirigir a canales de recaudación.
   - **Otra Consulta:** Para consultar otro contrato o regresar al menú.
2. **Si no tiene deuda:**
   - Confirmación de estar al día.
   - Opción de consultar últimos pagos o volver al menú.

## Salida

- **Consultar otro contrato TRB:** Transfiere a [_ 1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md).
- **Pagar Factura:** Transfiere a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).
- **Otra Consulta / Salir:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) o [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md).
- **En caso de error del servicio:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
