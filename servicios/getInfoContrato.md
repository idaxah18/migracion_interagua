# getInfoContrato

Obtiene la información de un contrato: producto, cliente, identificación del beneficiario y dirección.

Jelou: tool **2937** (`get-info-contrato`).

## URL

Producción:

```
http://192.168.10.155/api-services-int/api/services
```

Desarrollo:

```
http://192.168.146.2:93/api/services
```

La tool elige la URL con el input `env` (`Produccion` o `Desarrollo`).

## Headers

| Header         | Valor              |
| -------------- | ------------------ |
| `apikey`       | `30072026`         |
| `Content-Type` | `application/json` |

## Request

`numeroContrato` es obligatorio. `identificacion` es opcional. En la consulta por contrato, el flujo envía el mismo número en los dos campos.

```json
{
  "accion": "getInfoContrato",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}"
}
```

## Response — éxito

HTTP **200**. La tool expone el cuerpo en la salida `getInfoContrato`. Los flujos toman el primer elemento y leen producto, cliente y dirección.

```json
[
  {
    "ID_CONTRATO": "609580",
    "ID_PRODUCTO": "117952",
    "ID_CLIENTE": "128062",
    "IDENTIFICACION_BENEF": "0904977881",
    "NOMBRE_BENEFICIARIO": "LUZ MERCEDES NAULA TENEPAGUAY",
    "DIRECCIONTRADICIONAL": "SECTOR CISNE II",
    "ADDRESS_ID": "3812593"
  }
]
```

Códigos de negocio usados en flujos:

| Código   | Significado |
| -------- | ----------- |
| `20082`  | La cédula ingresada es incorrecta |
| `801543` | El contrato es incorrecto o no tiene contratos activos |

Si no hay registros, el flujo indica que no encontró información. Si el contrato no trae `ADDRESS_ID`, indica que no hay dirección registrada.

## Response — error

HTTP **409** cuando el servicio responde con error de negocio. Otro estado distinto de 200 se trata como falla del servicio.

```json
{
  "notice": "Se produjo un error.",
  "exception": "El servicio no respondió satisfactoriamente. [Status:422]",
  "responseFromService": {
    "errorMessage": "El contrato ingresado es incorrecto, o no tiene contratos activos.",
    "errorCode": 801543
  }
}
```
