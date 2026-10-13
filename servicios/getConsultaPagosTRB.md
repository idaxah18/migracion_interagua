# getConsultaPagosTRB

Consulta el historial de pagos TRB (Tasa de Recolección de Basura) de un contrato.

Jelou: tool **5955** (`get-consulta-pagos-trb`).

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
| `APIKEY`       | `21082026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "getConsultaPagosTRB",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

## Response — éxito

HTTP **200**. Lista de pagos TRB (salida Jelou `pagos`).

```json
[
  {
    "valor_ultimo_pago": "12.50",
    "fecha_ultimo_pago": "15/08/2026"
  }
]
```

Los flujos usan el primer registro para último pago y el listado `txt_pagos`.

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
