# getSolicitud

Consulta el detalle de una solicitud/trámite por su número.

Jelou: tool **3036** (`get-solicitud`).

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
  "accion": "getSolicitud",
  "identificacion": "{{identificacion}}",
  "numeroSolicitud": "{{numeroSolicitud}}"
}
```

## Response — éxito

HTTP **200**. Detalle del trámite (salida Jelou `getSolicitud`).

```json
{
  "NUMERO_SOLICITUD": "12345678",
  "ESTADO": "En proceso",
  "DESCRIPCION": "Reclamo alta facturación",
  "FECHA_REGISTRO": "10/10/2026"
}
```

## Response — error

| Código | Significado en flujos |
| ------ | --------------------- |
| `902000` | No hay respuesta configurada |
| `3303`, `801543` | Contrato/solicitud inválidos |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontraron datos para la solicitud consultada.",
    "errorCode": 902000
  }
}
```
