# getParametroFinanciacionGeneracionConvenio

Obtiene parámetros del plan de financiación antes de generar el convenio de pago.

Jelou: tool **2921** (`get-parametro-financiacion-generacion-convenio`).

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
  "accion": "getParametroFinanciacionGeneracionConvenio",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

## Response — éxito

HTTP **200**. Parámetros de financiación (salida Jelou `getParametroGeneracion`).

```json
{
  "PLAN_FINANCIACION": "12345",
  "FINA_DEUDA_VENCIDA": "S"
}
```

Los flujos guardan `idPlanAcuerdoPago` y `financiaDeudaVencida` en memoria.

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
