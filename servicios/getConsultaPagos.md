# getConsultaPagos

Consulta el historial de pagos de un contrato de agua potable.

Jelou: tool **5465** (`get-consulta-pagos`).

## URL

Producción:

```
http://192.168.10.157/api-services-int/api/services
```

Desarrollo:

```
http://192.168.146.2:93/api/services
```

## Headers

| Header         | Valor              |
| -------------- | ------------------ |
| `apikey`       | `30072026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "getConsultaPagos",
  "identificacion": "{{identificacion}}",
  "numeroContrato": {{numeroContrato}}
}
```

## Response — éxito

HTTP **200**. Lista de pagos (salida Jelou `pagos`).

```json
[
  {
    "VALOR_PAGADO": "39.39",
    "FECHA_PAGO": "20/09/2026",
    "BANCO_CANAL": "Transferencia web"
  }
]
```

Los flujos toman el primer registro como último pago (`valor_ultimo_pago`, `fecha_ultimo_pago`) y arman el texto `txt_pagos`.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Descripción del error",
    "errorCode": 801543
  }
}
```
