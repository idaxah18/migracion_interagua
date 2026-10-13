# getSolicitudVentaServiciosIngenieria

Crea la solicitud de venta de servicios de ingeniería (pozo séptico, trampa de grasa, etc.).

Jelou: tool **6439** (`get-solicitud-venta-servicios-ingenieria`).

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
  "accion": "getSolicitudVentaServiciosIngenieria",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{contrato}}",
  "nombreBeneficiario": "{{nombreBeneficiario}}",
  "correoElectronico": "{{correoElectronico}}",
  "telefonoContacto": "{{telefonoContacto}}",
  "identificacionBeneficiario": "{{identificacionBeneficiario}}",
  "direccionBeneficiario": "{{direccionBeneficiario}}",
  "actividad": "{{actividad}}",
  "comentario": "{{comentario}}",
  "comentario2": "{{comentario2}}",
  "comentario3": "{{comentario3}}",
  "comentario4": "",
  "comentario5": "",
  "comentario6": "",
  "comentario7": "",
  "comentario8": "",
  "comentario9": "",
  "comentario10": ""
}
```

En flujos, `comentario2`/`comentario3` suelen llevar código de tarifa y monto (ej. `PE_VTA_INGE_DESC_32`, `112.33`).

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

Negocio: salida `data_er`. Técnico: salida `error`. Código `20201` en flujos implica pago previo.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se pudo registrar la solicitud.",
    "errorCode": 20201
  }
}
```
