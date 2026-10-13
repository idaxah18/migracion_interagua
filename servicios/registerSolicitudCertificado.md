# registerSolicitudCertificado

Registra solicitud de certificado administrativo (ej. certificado de no adeudar).

Jelou: tool **6470** (`register-solicitud-certificado`).

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
  "accion": "registerSolicitudCertificado",
  "identificacion": "{{identificacion}}",
  "codigoProducto": "{{producto}}",
  "codigoCliente": "{{contact_id}}",
  "codigoDireccion": "{{address_id}}",
  "comentario": "{{comentario}}",
  "codigoTipoRecepcion": "{{canal}}"
}
```

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

Códigos usados en flujos:

| Código | Significado |
| ------ | ----------- |
| `41` | Deuda pendiente con la empresa |
| `1` | Solicitud ya registrada |
| `20201` | Requiere cancelación de valores |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Para solicitar este servicio Ud debe regularizar su deuda.",
    "errorCode": 41
  }
}
```
