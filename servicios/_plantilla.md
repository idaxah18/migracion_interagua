# Nombre del servicio

> **Archivo:** `{nombre-del-servicio}.md` — usar el nombre del servicio en minúsculas, palabras separadas por guiones (ej. `get-info-contrato.md`).

## URL

```
https://ejemplo.interagua/api/services
```

## Headers

| Header         | Valor              |
| -------------- | ------------------ |
| `Content-Type` | `application/json` |
| `apikey`       | `{apikey}`         |

## Request

```json
{
  "accion": "nombreAccion",
  "identificacion": "<valor>",
  "numeroContrato": "<valor>"
}
```

## Response — éxito

```json
{
  "codigo": "0",
  "mensaje": "OK",
  "data": {}
}
```

## Response — error

```json
{
  "codigo": "801543",
  "mensaje": "Descripción del error"
}
```
