# getSolicitudPorContrato

Lista solicitudes/trámites recientes asociados a un contrato.

Jelou: tool **3035** (`get-solicitud-por-contrato`).

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
  "accion": "getSolicitudPorContrato",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}",
  "cantidadRegistros": "{{cantidadRegistros}}"
}
```

Ejemplo en flujos: `cantidadRegistros` = `"20"`.

## Response — éxito

HTTP **200**. Listado de solicitudes (salida Jelou `getSolicitudes`).

```json
[
  {
    "NUMERO_SOLICITUD": "12345678",
    "TIPO_SOLICITUD": "Reclamo facturación",
    "ESTADO": "En proceso"
  }
]
```

## Response — error

| Código | Significado en flujos |
| ------ | --------------------- |
| `902000` | No hay respuesta configurada para la solicitud |
| `3303`, `801543` | Contrato incorrecto o sin contratos activos |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontraron solicitudes para el contrato consultado.",
    "errorCode": 0
  }
}
```
