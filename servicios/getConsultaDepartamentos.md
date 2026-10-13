# getConsultaDepartamentos

Consulta departamentos/unidades para inspección según tipo de servicio (venta servicios).

Jelou: tool **6418** (`get-consulta-departamentos`).

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
  "accion": "getConsultaDepartamentos",
  "identificacion": "{{identificacion}}",
  "idTipoInspeccion": "{{id_tipo_inspeccion}}"
}
```

## Response — éxito

HTTP **200**. Departamentos (salida Jelou `departamentos`).

```json
[
  {
    "ID_DEPARTAMENTO": "1",
    "DESCRIPCION": "Departamento A"
  }
]
```

## Response — error

Sin resultados: salida `sin_departamentos`.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No se encontraron departamentos.",
    "errorCode": 0
  }
}
```
