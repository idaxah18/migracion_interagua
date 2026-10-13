# getDetalleFacturaReclamo

Lista facturas emitidas del contrato elegibles para reclamo de alta facturación.

Jelou: tool **2961** (`get-detalle-factura-reclamo`).

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
  "accion": "getDetalleFacturaReclamo",
  "identificacion": "{{identificacion}}",
  "numeroContrato": {{numeroContrato}}
}
```

## Response — éxito

HTTP **200**. Arreglo de facturas (salida Jelou `getDetalleFacturaReclamo`).

```json
[
  {
    "FACTCODI": "101227510",
    "MES": "10",
    "ANIO": "2026",
    "FECHA_VENCIMIENTO": "23/10/2026",
    "VALOR": "39.39"
  }
]
```

## Response — error

| Código | Significado en flujos |
| ------ | --------------------- |
| `111112`, `801543`, `3303` | Contrato incorrecto o sin contratos activos |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró facturas para el contrato ingresado.",
    "errorCode": 0
  }
}
```
