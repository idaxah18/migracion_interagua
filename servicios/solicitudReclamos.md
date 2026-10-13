# solicitudReclamos

Envía por correo el PDF con la resolución de una solicitud/reclamo consultado.

Jelou: tool **6473** (`solicitud-reclamos`).

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
| `APIKEY`       | `01092026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "solicitudReclamos",
  "identificacion": "{{identificacion}}",
  "numeroSolicitud": "{{solicitud}}",
  "correo1": "{{correo1}}",
  "correo2": "{{correo2}}"
}
```

## Response — éxito

HTTP **200** (salida Jelou `data_ex`).

```json
{
  "mensaje": "PDF enviado al correo seleccionado"
}
```

## Response — error

Salida `data_er` o `error`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Ha ocurrido un error al enviar el PDF a tu correo.",
    "errorCode": 0
  }
}
```
