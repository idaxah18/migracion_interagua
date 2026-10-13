# registerDanoProdducto

Registra un reclamo técnico de daño a producto / orden de trabajo en campo.

Jelou: tool **2941** (`register-dano-producto`). La acción en API conserva el typo `Prodducto`.

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
  "accion": "registerDanoProdducto",
  "identificacion": "{{identificacion}}",
  "codigoTipoPaquete": {{codigoTipoPaquete}},
  "codigoProducto": {{codigoProducto}},
  "codigoCliente": {{codigoCliente}},
  "codigoDireccion": {{codigoDireccion}},
  "codigoCausal": {{codigoCausal}},
  "comentario": "{{comentario}}",
  "codigoDireccionProducto": {{codigoDireccionProducto}}
}
```

## Response — éxito

HTTP **200**. Registro creado (salida Jelou `danoProducto`).

```json
{
  "PACKAGE_ID": "12345678"
}
```

Los flujos guardan `numeroSolicitud` desde `PACKAGE_ID`.

## Response — error

Errores de negocio mapeados en flujos (`mappingError`) o respuesta vacía. Ejemplo genérico:

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Estimado cliente no hemos podido registrar tu reclamo.",
    "errorCode": 0
  }
}
```
