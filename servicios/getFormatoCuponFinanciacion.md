# getFormatoCuponFinanciacion

Obtiene número de cupón y fecha límite de pago del convenio generado.

Jelou: tool **2924** (`get-formato-cupon-financiacion`).

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
  "accion": "getFormatoCuponFinanciacion",
  "identificacion": "{{identificacion}}",
  "numeroSolicitud": "{{numeroSolicitud}}"
}
```

## Response — éxito

HTTP **200**. Datos del cupón (salida Jelou `getFormatoCuponFinanciacion`).

```json
{
  "CUPON": "026-100-012345678",
  "FECHA_LIMITE": "23/10/2026"
}
```

Los flujos guardan `numeroCupon` (`CUPON`) y el texto de vigencia según `FECHA_LIMITE`.

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
