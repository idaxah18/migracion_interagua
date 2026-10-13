# getListadoTipoServicio

Catálogo de tipos de servicio de ingeniería disponibles para venta.

Jelou: tool **6417** (`get-listado-tipo-servicio`).

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
  "accion": "getListadoTipoServicio",
  "identificacion": "{{identificacion}}"
}
```

## Response — éxito

HTTP **200**. Servicios disponibles (salida Jelou `servicios`).

```json
[
  {
    "ID_TIPO_SERVICIO": "32",
    "DESCRIPCION": "Limpieza de pozo séptico"
  }
]
```

## Response — error

Sin catálogo: salida `sin_servicios`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No existen servicios disponibles por el momento.",
    "errorCode": 0
  }
}
```
