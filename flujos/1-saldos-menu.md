# 1 - Saldos - Menu

Presenta las opciones de consulta de deuda para que el cliente elija si desea revisar sus valores de consumo de Agua Potable o de la Tasa de Recolección de Basura (TRB).

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Autorización LOPD: Verificada.
- Parámetros de consentimiento: `{{$memory.paramsLODP}}` (si requiere registrarse en base de datos).
- Bandera de inserción: `{{$memory.requiredQuestion}}`.

## Paso 1. Selección del tipo de saldo

El bot envía una lista interactiva con el mensaje:

> ¿Qué saldo o pago deseas consultar?  
> Por favor selecciona una de las siguientes opciones:

Opciones presentadas:

1. **Agua Potable** (`saldo_agua`): Consulta de consumo y facturas de agua potable.
2. **TRB** (`saldo_trb`): Consulta de la Tasa de Recolección de Basura.
3. **Menú Principal**: Regresar al menú inicial de servicios.

## Paso 2. Registro de consentimiento LOPD (si aplica)

Al seleccionar una opción, si `{{$memory.requiredQuestion}}` es `true`, el flujo ejecuta en segundo plano el registro de la aceptación:

Servicio: **SavePoliticaDatos** (Tool ID `1545`)

Headers:

apikey = Bearer Token Jelou  
Content-type = application/json  

Endpoint:

`POST https://api.jelou.ai/v2/databases/1611/rows`

Request:

```json
{
  "canal": "{{$memory.canal}}",
  "fecha": "<fecha>",
  "hora": "<hora>",
  "userId": "<telefono>",
  "userName": "<nombre>",
  "identificacion": "<identificacion>"
}
```

## Salida

Según la opción seleccionada por el cliente:

- **Agua Potable:** Transfiere a [_ 1.1 - Saldos Consumo Agua - Inicio](./1-1-saldos-consumo-agua-inicio.md) (Skill ID `51071`).
- **TRB:** Transfiere a [_ 1.2 - Saldos TRB - Inicio](./1-2-saldos-trb-inicio.md) (Skill ID `51075`).
- **Menú Principal:** Transfiere a [_ 0.4 - Menú Principal - Opciones](./0-4-menu-principal-opciones.md) (Skill ID `51039`) o [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
