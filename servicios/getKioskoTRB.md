# getKioskoTRB

Lista contratos TRB e información del cliente por **número de contrato** o por **identificación** (cédula/RUC).

Jelou: tool **2896** (`get-kiosko-trb`).

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

La tool calcula `idTipoConsulta` según `search_by`: `1` = contrato, `2` = identificación.

```json
{
  "accion": "getKioskoTRB",
  "identificacion": "{{identificacion}}",
  "idTipoConsulta": "{{idTipoConsulta}}",
  "idConsulta": "{{idConsulta}}"
}
```

| Campo | Uso |
| ----- | --- |
| `identificacion` | Identificación del titular (cuando aplica) |
| `idConsulta` | Valor buscado (número de contrato o cédula/RUC) |
| `idTipoConsulta` | `1` consulta por contrato · `2` consulta por identificación |

## Response — éxito

HTTP **200**. Arreglo de contratos TRB (salida Jelou `getKioskoTRB`).

```json
[
  {
    "ID_CONTRATO": "609580",
    "IDENTIFICACION_BENEF": "0904977881",
    "NOMBRE_BENEFICIARIO": "LUZ MERCEDES NAULA TENEPAGUAY",
    "DIRECCIONTRADICIONAL": "SECTOR CISNE II",
    "ADDRESS_ID": "3812593"
  }
]
```

Códigos de negocio usados en flujos:

| Código  | Significado |
| ------- | ----------- |
| `913194` | No existen contratos asociados para la identificación |
| `801543` | Contrato incorrecto o sin contratos activos |

## Response — error

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "No existen contratos asociados para el número de identificación.",
    "errorCode": 913194
  }
}
```
