# 1.1 - Saldos Consumo Agua - Confirmación

Confirma el contrato, consulta el estado de cuenta y los últimos pagos, y según la deuda ofrece convenio, pago en línea o el menú.

## Datos de entrada

- `Contrato`: cuenta elegida en [Consulta de contratos](./consulta-contratos-inicio.md). Trae `ID_CONTRATO` y `DIRECCIONTRADICIONAL`.
- `identificacion`: cédula o RUC usada en esa consulta, o `IDENTIFICACION_BENEF` del contrato.
- `contexto`: `saldos`, asignado en [1 - Saldos - Inicio](./1-saldos-inicio.md).

Si se abre sin venir de la consulta de contratos: [0 - Inicio](./0-inicio.md).

## 1. Confirmación del contrato

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:
>
> *Contrato No. {Contrato.ID_CONTRATO} - {Contrato.DIRECCIONTRADICIONAL}*
>
> ¿Estás de acuerdo con la información ingresada?

Botones: **Si** · **No**

Si no elige una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

- **No:** vuelve a [1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md).
- **Si:** consulta el estado de cuenta.

## 2. Estado de cuenta

Servicio: [getEstadoCuenta](../servicios/getEstadoCuenta.md).

Si el servicio no responde: [Mensaje de error de servicio](./mensaje-error-servicio.md).

Con la respuesta:

| Condición | Mensaje (`msm_error`) |
| --------- | --------------------- |
| `errorCode` `801543` | El contrato ingresado es incorrecto, o no tiene contratos activos. |
| `errorCode` `20082` | La cédula ingresada es incorrecta. |
| `errorCode` `528` | El contrato ingresado no pertenece al número de identificación registrado en INTERAGUA. |
| Otro error | !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde. |
| Sin datos | No se encontró información para el contrato ingresado |

Si hay error: [Mensaje de error personalizado](./mensaje-error-personalizado.md).

Si es válido:

- Se toma el primer registro y se guarda en `dataEstadoCuenta` (86000 segundos).
- En `dataEstadoCuenta` se deja solo la fecha (sin hora) en `FECHA_EMISION` y `FECHA_VENC_FACT`.
- `tieneDeuda` queda en verdadero si `dataEstadoCuenta.DEUDA_PENDIENTE` es mayor que 0.

## 3. Últimos pagos

Servicio: [getConsultaPagos](../servicios/getConsultaPagos.md).

Por defecto, antes de completar esta consulta:

- `txt_pagos` = `No posee pagos anteriores`
- `fecha_ultimo_pago` = `No posee pagos anteriores`
- `valor_ultimo_pago` = `No posee pagos anteriores`

Si el servicio falla con esos valores por defecto, igual se muestra el estado de cuenta. Si falla de otro modo: [Mensaje de error de servicio](./mensaje-error-servicio.md).

Si hay pagos:

| Condición | Mensaje |
| --------- | ------- |
| `errorCode` `801543` | El contrato ingresado es incorrecto, o no tiene contratos activos. |
| Otro error | !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde. |
| Lista vacía | No se encontraron pagos registrados. |

Si hay pagos, el primero es el más reciente. De ahí salen `valor_ultimo_pago` (`VALOR_PAGADO`) y `fecha_ultimo_pago` (fecha de `FECHA_PAGO` sin hora). `txt_pagos` se arma así:

`N - Fecha - $Valor - DESCBANCOCLIENTE`

## 4. Resumen

> Saldo actual: *${dataEstadoCuenta.DEUDA_PENDIENTE}*
> Planillas adeudadas: *{dataEstadoCuenta.ACCOUNT_WITH_BALANCE}*
> Fecha de vencimiento: *{dataEstadoCuenta.FECHA_VENC_FACT}*
> Fecha de emisión de la última factura: *{dataEstadoCuenta.FECHA_EMISION}*
> Último pago realizado: $*{valor_ultimo_pago}*
> Fecha último pago realizado: *{fecha_ultimo_pago}*

## 5. Si no hay deuda

> ¿Quieres consultar los últimos pagos realizados?

Botones: **Si** · **No**

- **Si:** muestra `{txt_pagos}` y pasa a [Atención adicional](./atencion-adicional.md).
- **No:** [Atención adicional](./atencion-adicional.md).

## 6. Si hay deuda

Servicio: [getInfoSimulacionConvenio](../servicios/getInfoSimulacionConvenio.md).

| Condición | Mensaje |
| --------- | ------- |
| `errorCode` `3303` | Debe ingresar la información requerida por el contrato. |
| Otro error | !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde. |

`aplica_simulacion` queda en verdadero solo si la respuesta de [getInfoSimulacionConvenio](../servicios/getInfoSimulacionConvenio.md) trae `Codigo` `0`, hay datos, hay deuda y `APLICA_SIMULACION_VIRTUAL` es `S`. Si el servicio falla: [Atención adicional](./atencion-adicional.md).

### 6.1 Aplica simulación de convenio

Lista (botón **Opciones**):

> ¿Qué deseas realizar?
>
> Por favor selecciona una de las siguientes opciones

- **Últimos pagos:** muestra `{txt_pagos}` y luego:

> ¿Que deseas realizar?

Botones: **Convenio de pago** · **Pagar en línea** · **Menú principal**

- **Convenio de pago:** muestra `Modulo convenio` y vuelve a preguntar:

> ¿Que deseas realizar?

Botones: **Convenio de pago** · **Pagar en línea** · **Menú principal**

- **Pagar en línea:** [3 - Pago de planilla](./3-pago-de-planilla-inicio.md).
- **Menú principal:** [Menú principal - Bienvenida](./0-3-menu-principal-bienvenida.md).

### 6.2 No aplica simulación

> ¿Que deseas realizar?

Botones: **Últimos pagos** · **Pagar en línea** · **Menú principal**

- **Últimos pagos:** muestra `{txt_pagos}` y luego:

> ¿Que deseas realizar?

Botones: **Pagar en línea** · **Menú principal**

- **Pagar en línea:** [3 - Pago de planilla](./3-pago-de-planilla-inicio.md).
- **Menú principal:** [Menú principal - Bienvenida](./0-3-menu-principal-bienvenida.md).
