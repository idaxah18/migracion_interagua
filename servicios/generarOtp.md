# generarOtp

Genera y envía un código OTP al correo del cliente.

Jelou: tool **6436** (`generar-otp`).

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
| `apikey`       | `01092026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "generarOtp",
  "identificacion": "{{identificacion}}",
  "correoUsuario": "{{correo}}",
  "numeroContrato": "{{contrato}}"
}
```

## Response — éxito

HTTP **200**. Código generado/enviado (salida Jelou `codigo`).

```json
{
  "mensaje": "OTP enviado correctamente",
  "estado": "OK"
}
```

## Response — error

Salida Jelou `error`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No fue posible enviar el código OTP.",
    "errorCode": 0
  }
}
```
