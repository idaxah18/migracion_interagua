# validarOtp

Valida el código OTP ingresado por el cliente.

Jelou: tool **6437** (`validar-otp`).

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
  "accion": "validarOtp",
  "identificacion": "{{identificacion}}",
  "codigoOtp": "{{codigo_otp}}",
  "correoUsuario": "{{correo}}",
  "numeroContrato": "{{contrato}}"
}
```

## Response — éxito

HTTP **200** (salida Jelou `mensaje_otp_ex`).

```json
{
  "valido": true,
  "mensaje": "Código verificado"
}
```

## Response — error

Código incorrecto o expirado (salida Jelou `mensaje_otp_er`).

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Código OTP inválido.",
    "errorCode": 0
  }
}
```
