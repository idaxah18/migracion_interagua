# getValorCuotaAproximada

Calcula cuota inicial y cuota mensual aproximada para un convenio de pago simulado.

Jelou: tool **2916** (`get-valor-cuota-aproximada`).

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
  "accion": "getValorCuotaAproximada",
  "identificacion": "{{identificacion}}",
  "cuotaInicial": "{{cuotaInicial}}",
  "deudaPendiente": "{{deudaPendiente}}",
  "mesesFinanciar": "{{mesesFinanciar}}"
}
```

| Campo | Uso |
| ----- | --- |
| `cuotaInicial` | Valor calculado: `deudaPendiente × porcentaje / 100` |
| `deudaPendiente` | Deuda total a financiar (`valorDeuda`) |
| `mesesFinanciar` | Plazo en meses elegido |

## Response — éxito

HTTP **200**. Liquidación tentativa (salida Jelou `getValorCuotaAproximada`).

```json
{
  "CUOTA_INICIAL": "15.00",
  "CUOTA_APROXIMADA": "22.50"
}
```

Los flujos guardan `cuotaInicial`, `cuotaAproximada` y `dataCuotaAproximada` en memoria.

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró valor de cuota para el plazo y deuda indicado.",
    "errorCode": 0
  }
}
```
