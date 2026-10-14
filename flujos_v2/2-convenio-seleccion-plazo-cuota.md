# Selección de plazo y cuota

Elige el porcentaje de cuota inicial y los meses de financiamiento de un convenio.

Se invoca desde [Convenio de pago](./2-convenio-de-pago.md) en dos momentos, con la misma secuencia:

- La primera vez, después de confirmar que el contrato es elegible.
- De nuevo, si en la simulación el cliente quiere cambiar plazo o cuota.

No confirma el contrato, no vuelve a consultar la deuda y no emite el convenio.

## Datos de entrada

- Identificación del cliente.
- Deuda pendiente del contrato.

## 1. Porcentaje de cuota inicial

Servicio: [getListadoPorcentajeCuotaInicial](../servicios/getListadoPorcentajeCuotaInicial.md).

Sin opciones: *No se encontró información de porcentajes de cuota inicial.* Se cierra con [Mensaje de error personalizado](./mensaje-error-personalizado.md).

Con opciones:

> Ok, te comentamos que tu deuda actual es de ${deudaPendiente}
>
> Elige el porcentaje de tu cuota inicial.

Cada opción es un porcentaje (10%, 20%, 30%, …). La elección queda en `porcentaje`.

## 2. Plazo en meses

Servicio: [getListadoMesesFinanciacion](../servicios/getListadoMesesFinanciacion.md).

Sin opciones: *No se encontró información para el contrato ingresado.* Se cierra con [Mensaje de error personalizado](./mensaje-error-personalizado.md).

Con opciones:

> A cuantos meses deseas financiar tu convenio.

Cada opción es un plazo habilitado (3, 6, 12 meses, …). La elección queda en `mesesFinanciar`.

## Salida

Devuelve `porcentaje` y `mesesFinanciar` a [Convenio de pago](./2-convenio-de-pago.md#5-simulación) para calcular de nuevo la cuota inicial y la cuota mensual.
