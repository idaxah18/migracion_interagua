# getListadoCorreoCliente

Lista correos electrónicos registrados en la ficha del contrato (OTP, certificados, envío de PDFs).

Jelou: tool **6419** (`get-listado-correo-cliente`).

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
  "accion": "getListadoCorreoCliente",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

## Response — éxito

HTTP **200**. Listado de correos (salida Jelou `correos`).

```json
[
  { "CORREO_ELECTRONICO": "cliente@correo.com" }
]
```

## Response — error

Sin correos: salida `sin_correos`. Error técnico: salida `error`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró un correo electrónico asociado a tu contrato.",
    "errorCode": 0
  }
}
```
