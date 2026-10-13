# registroSolicitudSuscripcion

Registra solicitud de certificado de suscripción del servicio.

Jelou: tool **6472** (`registro-solicitud-suscripcion`).

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
  "accion": "registroSolicitudSuscripcion",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{contrato}}",
  "nombreBeneficiario": "{{nombreBeneficiario}}",
  "correoElectronico": "{{correoElectronico}}",
  "telefonoContacto": "{{telefonoContacto}}",
  "identificacionBeneficiario": "{{identificacionBeneficiario}}",
  "direccionBeneficiario": "{{direccionBeneficiario}}",
  "actividad": "{{actividad}}",
  "comentario": "{{comentario}}",
  "comentario2": "",
  "comentario3": "",
  "comentario4": "",
  "comentario5": "",
  "comentario6": "",
  "comentario7": "",
  "comentario8": "",
  "comentario9": "",
  "comentario10": ""
}
```

En flujos, `actividad` suele ser `"593650"` para certificado de suscripción.

## Response — éxito

HTTP **200** (salida Jelou `data_ex`).

```json
{
  "data": {
    "PACKAGE_ID": "12345678"
  }
}
```

## Response — error

Salida `data_er` o `error`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se pudo registrar la solicitud.",
    "errorCode": 0
  }
}
```
