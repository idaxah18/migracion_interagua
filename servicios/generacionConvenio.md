# generacionConvenio

Emite formalmente el convenio de pago en el sistema comercial.

Jelou: tool **3054** (`generacion-convenio`).

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

La tool fija `idTipoFinanciacion` en `"S"`.

```json
{
  "accion": "generacionConvenio",
  "identificacion": "{{identificacion}}",
  "codigoCliente": "{{codigoCliente}}",
  "codigoDireccionContrato": "{{codigoDireccionContrato}}",
  "codigoAplicante": "{{codigoAplicante}}",
  "comentario": "{{comentario}}",
  "idTipoFinanciacion": "S",
  "numeroContrato": "{{numeroContrato}}",
  "idPlanAcuerdoPago": "{{idPlanAcuerdoPago}}",
  "valorCuotaInicial": "{{valorCuotaInicial}}",
  "fechaFinanciacion": "{{fechaFinanciacion}}",
  "numeroCuotas": "{{numeroCuotas}}",
  "codigoProducto": "{{codigoProducto}}",
  "valorDeudaPendiente": "{{valorDeudaPendiente}}"
}
```

| Campo | Uso típico en flujos |
| ----- | -------------------- |
| `comentario` | `WH <teléfono> Financiacion de deuda` |
| `fechaFinanciacion` | Fecha actual Ecuador `YYYY-MM-DD` |
| `numeroCuotas` | Plazo `mesesFinanciar` |
| `valorCuotaInicial` | Cuota inicial acordada |
| `codigoAplicante` | Código del aplicante (según configuración del flujo) |

## Response — éxito

HTTP **200**. Resultado de generación (salida Jelou `getGeneracionResult`).

```json
{
  "requestOut": {
    "requestId": "987654321"
  }
}
```

Los flujos guardan `numeroSolicitud` desde `requestId`.

## Response — error

Códigos usados en flujos:

| Código | Mensaje al cliente |
| ------ | ------------------ |
| `901705` | No hay deuda financiable |
| `903357` | Reclamo en proceso — contactar al 134 |
| `919317` | Contrato no aplica — contactar al 134 |

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No hay deuda financiable.",
    "errorCode": 901705
  }
}
```
