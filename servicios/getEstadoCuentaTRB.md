# getEstadoCuentaTRB

Consulta el estado de cuenta de la Tasa de Recolección de Basura (TRB) para un contrato e identificación dados.

Jelou: tool **2901** (`get-estado-cuenta-trb`).

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
| `APIKEY`       | `21082026`         |
| `Content-Type` | `application/json` |

## Request

```json
{
  "accion": "getEstadoCuentaTRB",
  "numeroContrato": "{{numeroContrato}}",
  "identificacion": "{{identificacion}}"
}
```

## Response — éxito

HTTP **200**. El cuerpo es el payload del servicio (la tool lo expone en la salida `getEstadoCuentaTRB`).

```json
[
  {
    "ID_CONTRATO": "609580",
    "IDENTIFICACION_BENEF": "0904977881",
    "NOMBRE_BENEFICIARIO": "LUZ MERCEDES NAULA TENEPAGUAY",
    "DEUDA_PENDIENTE": "12.50",
    "ACCOUNT_WITH_BALANCE": "1",
    "FECHA_EMISION": "13/10/2026 08:56:46",
    "FECHA_VENC_FACT": "23/10/2026 23:59:59",
    "DIRECCIONTRADICIONAL": "SECTOR CISNE II",
    "ADDRESS_ID": "3812593"
  }
]
```

Códigos de negocio usados en flujos (campo `errorCode` / `responseFromService` cuando aplica):

| Código  | Significado |
| ------- | ----------- |
| `801543` | Contrato incorrecto o sin contratos activos |
| `20082` | Cédula incorrecta |
| `528` | El contrato no pertenece a la identificación registrada |

## Response — error

HTTP distinto de **200**, o HTTP **409** con detalle de negocio:

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "Contrato no pertenece al número de Identificación registrado en INTERAGUA.",
    "errorCode": 528
  }
}
```
