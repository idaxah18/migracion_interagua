# registroReclamosFacturacion

Registra formalmente el reclamo comercial de alta facturación con las facturas y cargos seleccionados.

Jelou: tool **3009** (`registro-reclamos-facturacion`).

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

`listaFacturas` es un objeto JSON (no string) con la estructura armada en el flujo.

```json
{
  "accion": "registroReclamosFacturacion",
  "identificacion": "{{identificacion}}",
  "codigoCliente": "{{codigoCliente}}",
  "codigoCausal": "{{codigoCausal}}",
  "codigoDireccionContrato": "{{codigoDireccionContrato}}",
  "numeroContrato": "{{numeroContrato}}",
  "comentario": "{{comentario}}",
  "listaFacturas": {{listaFacturas}}
}
```

## Response — éxito

HTTP **200** (salida Jelou `getRegistroReclamo`).

```json
{
  "REQUEST_ID": "987654321"
}
```

Los flujos guardan `numeroSolicitud` desde `REQUEST_ID`.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Estimado cliente no hemos podido registrar tu reclamo.",
    "errorCode": 0
  }
}
```
