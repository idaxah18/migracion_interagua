# 1.2 - Saldos TRB - Confirmación

Confirma el contrato TRB, muestra el saldo y los últimos pagos, y ofrece pagar o volver al menú.

## Datos de entrada

- `Contrato`: cuenta elegida en [1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md). Trae `ID_CONTRATO` y `DIRECCION_CORTA`.
- `identificacion`: cédula o RUC escrita por el cliente, o `IDENTIFICACION` del primer contrato de [getKioskoTRB](../servicios/getKioskoTRB.md) cuando busca por contrato.

Si se abre sin el contrato: [0 - Inicio](./0-inicio.md).

## 1. Confirmación del contrato

> Por favor confírmanos si está correcto el número de contrato y dirección:
>
> *Contrato No. {Contrato.ID_CONTRATO} - {Contrato.DIRECCION_CORTA}*
>
> ¿Estás de acuerdo con la información ingresada?

Botones: **Si** · **No**

Si no elige una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

- **No:** vuelve a [1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md).
- **Si:** consulta el estado de cuenta TRB.

## 2. Estado de cuenta TRB

Servicio: [getEstadoCuentaTRB](../servicios/getEstadoCuentaTRB.md).

Si el servicio no responde: [Mensaje de error de servicio](./mensaje-error-servicio.md).

| Condición | Mensaje (`msm_error`) |
| --------- | --------------------- |
| `errorCode` `801543` | El contrato ingresado es incorrecto, o no tiene contratos activos. |
| `errorCode` `20082` | La cédula ingresada es incorrecta. |
| `errorCode` `528` | El contrato ingresado no pertenece al número de identificación registrado en INTERAGUA. |
| Otro error | !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde. |
| Sin datos | No se encontró información para el contrato ingresado |

Si hay error: [Mensaje de error personalizado](./mensaje-error-personalizado.md).

Si es válido:

- Se toma el primer registro y se guarda en `dataEstadoCuentaTRB` (86400 segundos).
- En `dataEstadoCuentaTRB` se deja solo la fecha (sin hora) en `FECHA_EMISION` y `FECHA_VENC_FACT`.
- `tieneDeuda` queda en verdadero si `dataEstadoCuentaTRB.DEUDA_PENDIENTE` es mayor que 0.

## 3. Últimos pagos TRB

Servicio: [getConsultaPagosTRB](../servicios/getConsultaPagosTRB.md).

Por defecto, si esta consulta no trae pagos:

- `txt_pagos` = `No posee pagos anteriores`
- `fecha_ultimo_pago` = `No posee pagos anteriores`
- `valor_ultimo_pago` = `No posee pagos anteriores`

Si el servicio falla con esos valores por defecto, igual se muestra el saldo. Si falla de otro modo: [Mensaje de error de servicio](./mensaje-error-servicio.md).

Si hay pagos, el primero es el más reciente. De ahí salen `valor_ultimo_pago` (`VALOR_PAGADO`) y `fecha_ultimo_pago` (fecha de `FECHA_PAGO` sin hora). `txt_pagos` se arma así:

`N - Fecha - $Valor - DESCBANCOCLIENTE`

## 4. Resumen

> A continuación te presento el saldo de la *TRB* - “Tasa de Recolección de Basura”:
>
> *Saldo actual:* ${dataEstadoCuentaTRB.DEUDA_PENDIENTE}
> *Planillas adeudas:* {dataEstadoCuentaTRB.ACCOUNT_WITH_BALANCE}
> *Fecha de emisión de la última factura:* {dataEstadoCuentaTRB.FECHA_EMISION}
> *Fecha de vencimiento de la última factura:* {dataEstadoCuentaTRB.FECHA_VENC_FACT}
> Último pago realizado: $*{valor_ultimo_pago}*
> Fecha último pago realizado: *{fecha_ultimo_pago}*

Marcador `F Saldos TRB`.

## 5. Si hay deuda

> ¿Que deseas realizar?

Botones: **Últimos pagos** · **Pagar en línea** · **Menú principal**

- **Últimos pagos:** muestra el listado de pagos y luego:

> ¿Que deseas realizar?

Botones: **Pagar en línea** · **Menú principal**

- **Pagar en línea:** [3 - Pago de planilla](./3-pago-de-planilla-inicio.md).
- **Menú principal:** [Menú principal - Bienvenida](./0-3-menu-principal-bienvenida.md).

## 6. Si no hay deuda

> ¿Quieres consultar los últimos pagos realizados?

Botones: **Si** · **No** · **Menú principal**

- **Si:** muestra el listado de pagos y pasa a [Atención adicional](./atencion-adicional.md).
- **No:** [Atención adicional](./atencion-adicional.md).
- **Menú principal:** [Menú principal - Bienvenida](./0-3-menu-principal-bienvenida.md).
