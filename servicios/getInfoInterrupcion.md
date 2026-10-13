# getInfoInterrupcion

Consulta si existe interrupción o baja presión programada en el sector del contrato.

Jelou: tool **3025** (`get-info-interrupcion`).

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
  "accion": "getInfoInterrupcion",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}",
  "fechaInterrupcion": ""
}
```

## Response — éxito

HTTP **200** (salida Jelou `getInfoInterrupcion`).

Con afectación:

```json
{
  "has_interrupcion": true,
  "SECTOR_AFECTADO": "Ciudadela Norte",
  "TIEMPO_AFECTACION": "Restablecimiento estimado: 18:00"
}
```

Sin afectación:

```json
{
  "has_interrupcion": false
}
```

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
