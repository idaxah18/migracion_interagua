# getInfoSimulacionConvenio

Consulta deuda y elegibilidad del contrato para simular un convenio de pago.

Jelou: tool **2905** (`get-info-simulacion-convenio`).

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
  "accion": "getInfoSimulacionConvenio",
  "identificacion": "{{identificacion}}",
  "numeroContrato": {{numeroContrato}}
}
```

## Response — éxito

HTTP **200**. Datos del contrato para convenio (salida Jelou `getInfoSimulacionConvenio`).

```json
{
  "DEUDA_PENDIENTE": "150.00",
  "APLICA_SIMULACION_VIRTUAL": "S",
  "ID_PRODUCTO": "117952",
  "ID_CLIENTE": "128062",
  "ID_DIRECCION_CONTRATO": "3812593"
}
```

Reglas de negocio en flujos:

| Condición | Efecto |
| --------- | ------ |
| `DEUDA_PENDIENTE` ≤ 0 | Sin deuda vencida |
| `APLICA_SIMULACION_VIRTUAL` ≠ `S` | No elegible por canal virtual |
| Código `3303` | Falta información requerida del contrato |

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Debe ingresar la información requerida por el contrato.",
    "errorCode": 3303
  }
}
```
