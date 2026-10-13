# getDatosCliente

Obtiene datos del beneficiario y dirección del contrato (venta de servicios y otros trámites).

Jelou: tool **6440** (`get-datos-cliente`).

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
  "accion": "getDatosCliente",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

En la tool el input del contrato se llama `contrato`; se envía como `numeroContrato` en el JSON al API.

## Response — éxito

HTTP **200** (salida Jelou `datos_cliente`).

```json
{
  "NOMBRE_BENEFICIARIO": "LUZ MERCEDES NAULA TENEPAGUAY",
  "IDENTIFICACION_BENEF": "0904977881",
  "DIRECCIONTRADICIONAL": "SECTOR CISNE II"
}
```

## Response — error

Salida alternativa en tool: `sin_datos_cliente` / `error`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontró información del cliente.",
    "errorCode": 0
  }
}
```
