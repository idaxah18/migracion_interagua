# getListadoConceptosDeudaPorContrato

Lista los conceptos de deuda (`CARGCONC`) asociados al contrato para armar el convenio.

Jelou: tool **2922** (`get-listado-conceptos-deuda-por-contrato`).

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
| `APIKEY`       | `30072026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "getListadoConceptosDeudaPorContrato",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

## Response — éxito

HTTP **200**. Conceptos de deuda (salida Jelou `getListadoConceptosDeuda`).

```json
[
  { "CARGCONC": "101" },
  { "CARGCONC": "205" }
]
```

Los flujos transforman los códigos a `{ "string": ["101", "205"] }` y los guardan en `idConceptos`.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Descripción del error",
    "errorCode": 0
  }
}
```
