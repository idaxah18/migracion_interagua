# getConfirmacionGeneracionConvenio

Valida condiciones previas a la generación formal del convenio de pago (confirmación con deuda, porcentaje y plazo).

Jelou: tool **2920** (`get-confirmacion-generacion-convenio`).

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
  "accion": "getConfirmacionGeneracionConvenio",
  "identificacion": "{{identificacion}}",
  "valorDeuda": "{{valorDeuda}}",
  "porcentaje": "{{porcentaje}}",
  "meses": "{{meses}}"
}
```

## Response — éxito

HTTP **200**. Confirmación del servicio (salida Jelou `getDataConfirmacion`).

```json
{
  "APLICA_GENERACION_CONV": "S",
  "MENSAJE": "El convenio puede generarse."
}
```

Si `APLICA_GENERACION_CONV` ≠ `S`, los flujos bloquean la emisión y muestran contacto con call center (134).

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Tu cuenta no cumple con las condiciones para realizar un convenio de pago.",
    "errorCode": 0
  }
}
```
