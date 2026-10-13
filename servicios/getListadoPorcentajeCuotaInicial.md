# getListadoPorcentajeCuotaInicial

Obtiene los porcentajes de cuota inicial disponibles para un convenio según la deuda pendiente.

Jelou: tool **2914** (`get-listado-porcentaje-cuota-inicial`).

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
  "accion": "getListadoPorcentajeCuotaInicial",
  "identificacion": "{{identificacion}}",
  "valorDeudaPendiente": "{{valorDeudaPendiente}}"
}
```

## Response — éxito

HTTP **200**. Listado de porcentajes (salida Jelou `getListadoPorcentajeCuotaInicial`).

```json
[
  { "PORCENTAJE": "10" },
  { "PORCENTAJE": "20" },
  { "PORCENTAJE": "30" }
]
```

Los flujos muestran cada opción como `{{PORCENTAJE}}%` en lista interactiva.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró información de porcentajes de cuota inicial.",
    "errorCode": 0
  }
}
```
