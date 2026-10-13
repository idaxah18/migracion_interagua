# getEstadoCuenta

Consulta el estado de cuenta de consumo de agua potable para un contrato e identificación dados.

Jelou: tool **2887** (`get-estado-cuenta`).

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

La tool fija `tipoReferencia` en contexto con valor `1` antes de llamar al servicio.

```json
{
  "accion": "getEstadoCuenta",
  "identificacion": "{{identificacion}}",
  "numeroContrato": "{{numeroContrato}}",
  "tipoReferencia": "{{tipoReferencia}}"
}
```

## Response — éxito

HTTP **200**. El cuerpo es un arreglo con la información del contrato (la tool lo expone en la salida `getEstadoCuenta`).

```json
[
  {
    "ID_CLIENTE": "128062",
    "ID_CONTRATO": "609580",
    "DIRECCIONTRADICIONAL": "SECTOR CISNE II",
    "NOMBRE_BENEFICIARIO": "LUZ MERCEDES NAULA TENEPAGUAY",
    "IDENTIFICACION_BENEF": "0904977881",
    "NOMBRE_PROPIETARIO": "CAMPOVERDE JUAN\r",
    "IDENTIFICACION_PROPIETARIO": "******3530655",
    "ID_CLIENTE_PROPIETARIO": "1127502",
    "FACTCODI": "101227510",
    "FACTNUFI": "74964915",
    "FECHA_EMISION": "13/10/2026 08:56:46",
    "ID_PRODUCTO": "117952",
    "FECHA_VENC_FACT": "23/10/2026 23:59:59",
    "NUMDOC": "026-100-074964915",
    "TIPODOCU": "1",
    "RUCEMPRESA": "0992153563001",
    "DEUDA_PENDIENTE": "39.39",
    "ACCOUNT_WITH_BALANCE": "1",
    "DEF_BALANCE": "0",
    "CONSUMO": "34",
    "SBITEMTIPO": "Medidor de acueducto",
    "SBITEM": "SAPPEL",
    "SBSERIE": "D22BA0032216",
    "CATEGORIA": "RESIDENCIAL",
    "SUBCATEGORIA": "RESIDENCIAL",
    "ESTADO_CORTE": "N",
    "ULT_FECHA_PAGO": "20/09/2026",
    "ID_DIRECCION_CONTRATO": "3812593",
    "ADDRESS_ID": "3812593",
    "TELEFONO": "0995876421",
    "SALDO_RECLAMO": "0",
    "TELEFONO_AUX": "0995876421",
    "CORREO_ELECTRONICO": "documentoelectronico@interagua.com.ec",
    "CORREO_ELECTRONICO_ALTERNO": "documentoelectronico@interagua.com.ec",
    "TIPO_CLIENTE": "RESIDENCIALES",
    "LECTURA_ACTUAL": "1488",
    "LECTURA_ANTERIOR": "1454",
    "CONSUMO_ANTERIOR": "24",
    "CONSUMO_AHORRO": "0",
    "SECTOR_OPERATIVO": "38",
    "CICLO": "H",
    "TIENE_CONVENIO": "N",
    "APLICA_SIMULACION_VIRTUAL": "N",
    "VERIFICACION_CLIENTE": "0599;1057;6413;7881",
    "RESPUESTA_VERIFICA": "7881",
    "DIRECCION_COMPLETA": "CRISTOBAL COLON E CALLE 24 Y CALLE 23 DPTO. 26"
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
