# getListadoMesesFinanciacion

Obtiene los plazos en meses disponibles para financiar un convenio de pago según la deuda pendiente.

Jelou: tool **2915** (`get-listado-meses-de-financiacion`).

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
  "accion": "getListadoMesesFinanciacion",
  "identificacion": "{{identificacion}}",
  "valorDeudaPendiente": "{{valorDeudaPendiente}}"
}
```

## Response — éxito

HTTP **200**. Listado de meses (salida Jelou `getListadoMeses`).

```json
[
  { "MESES": "3" },
  { "MESES": "6" },
  { "MESES": "12" }
]
```

Los flujos muestran cada plazo en lista interactiva y guardan la elección en `mesesFinanciar`.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró información para el contrato ingresado.",
    "errorCode": 0
  }
}
```
