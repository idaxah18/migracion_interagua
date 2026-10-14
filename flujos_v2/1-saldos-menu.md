# 1 - Saldos - Menu

Presenta las opciones de consulta de deuda para que el cliente elija si desea revisar sus valores de consumo de Agua Potable o de la Tasa de Recolección de Basura (TRB).

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Autorización LOPD: Verificada.

## Paso 1. Selección del tipo de saldo

El bot envía una lista interactiva con el mensaje:

> ¿Qué saldo o pago deseas consultar?  
> Por favor selecciona una de las siguientes opciones:

Opciones presentadas:

1. **Agua Potable** (`saldo_agua`): Consulta de consumo y facturas de agua potable.
2. **TRB** (`saldo_trb`): Consulta de la Tasa de Recolección de Basura.
3. **Menú Principal**: Regresar al menú inicial de servicios.

## Salida

Según la opción seleccionada por el cliente:

- **Agua Potable:** Transfiere a [_ 1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md).
- **TRB:** Transfiere a [_ 1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md).
- **Menú Principal:** Transfiere a [_ 0.4 - Menú Principal - Opciones](./0-4-menu-principal-opciones.md) o [_ 0 - Inicio](./0-inicio.md).
