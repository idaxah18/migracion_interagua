# getDetalleCargos

Detalle de cargos/conceptos de una factura seleccionada para el reclamo.

Jelou: tool **2981** (`get-detalle-cargos`).

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
  "accion": "getDetalleCargos",
  "identificacion": "{{identificacion}}",
  "codigoFactura": {{codigoFactura}}
}
```

## Response — éxito

HTTP **200**. Cargos de la factura (salida Jelou `getDetalleCargos`).

```json
[
  {
    "ProductId": "117952",
    "AccountId": "1",
    "ConceptId": "100",
    "Sign": "+",
    "ConceptUnid": "1"
  }
]
```

## Response — error

| Código | Significado en flujos |
| ------ | --------------------- |
| `9061` | El número de solicitud no puede ser nulo |
| `900431` | Código de factura inválido |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "El código de la factura es inválido.",
    "errorCode": 900431
  }
}
```
