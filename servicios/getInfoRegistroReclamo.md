# getInfoRegistroReclamo

Obtiene datos maestros del contrato necesarios para registrar un reclamo de alta facturación.

Jelou: tool **2960** (`get-info-registro-reclamo`).

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
  "accion": "getInfoRegistroReclamo",
  "identificacion": "{{identificacion}}",
  "numeroContrato": {{numeroContrato}}
}
```

## Response — éxito

HTTP **200** (salida Jelou `getInfoRegistroReclamo`).

```json
{
  "ID_CLIENTE": "128062",
  "ID_DIRECCION_CONTRATO": "3812593"
}
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
    "errorMessage": "El contrato ingresado es incorrecto, o no tiene contratos activos.",
    "errorCode": 801543
  }
}
```
