# 1 - Saldos - Menú

Después del consentimiento, el cliente elige si consulta agua potable o TRB.

## Datos de entrada

- Teléfono de la sesión.
- Consentimiento LOPD ya verificado.
- `contexto`: `saldos`, asignado en [1 - Saldos - Inicio](./1-saldos-inicio.md).

## 1. Tipo de saldo

Lista (botón **Opciones**):

> ¿Qué saldo o pago deseas consultar?
>
> Por favor selecciona una de las siguientes opciones

| Opción | Descripción | Valor |
| ------ | ----------- | ----- |
| **Agua Potable** | | `saldo_agua` |
| **TRB** | Tasa de Recolección de Basura | `saldo_trb` |
| **Menú Principal** | Volver al menú | |

Si escribe algo que no es una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

Si el mensaje de la lista vence, se transfiere a [Utility - Opción no disponible](../flujos/utility-opcion-no-disponible.md).

## Salida

- `op_menu` = `saldo_agua`: [1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md).
- `op_menu` = `saldo_trb`: [1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md).
- **Menú Principal:** [0.4 - Menú principal - Opciones](./0-4-menu-principal-opciones.md).
- Si se abre sin el menú: [0 - Inicio](./0-inicio.md).
