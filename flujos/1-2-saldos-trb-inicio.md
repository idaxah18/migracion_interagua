# 1.2 - Saldos TRB - Inicio

Identifica el contrato o cédula del cliente para consultar el saldo y planillas pendientes de la Tasa de Recolección de Basura (TRB).

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Autorización LOPD: Aceptada y vigente.
- Contexto de navegación: `saldo_trb`.

## Paso 1. Selección de modo de consulta

El bot pregunta cómo desea buscar la información:

> ¿Deseas consultar tu saldo de la Tasa de Recolección de Basura con tu número de contrato o número de cédula?

Opciones:

1. **Contrato** (guarda modo `contrato`).
2. **Cédula o RUC** (guarda modo `identificacion`).

Si el usuario ingresa un texto no reconocido:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

## Paso 2. Consulta por número de contrato

Pregunta:

> Por favor ingresa tu número de contrato.

Validación previa de formato:
- Debe contener solo números y hasta un máximo de 7 dígitos.
- Si no cumple:
  > El número de contrato que ingresaste no es válido.
  > 
  > _Recuerda que solo debes ingresar números hasta máximo 7 dígitos._

Servicio: **Get Kiosko TRB** (Tool ID `2896`)

Headers:

apikey = 30072026  
Content-type = application/json  

Request:

```json
{
  "accion": "getKioskoTRB",
  "identificacion": "<contrato ingresado>",
  "numeroContrato": "<contrato ingresado>"
}
```

Reglas de respuesta del servicio:

1. Si la respuesta trae error, se evalúa el código:
   - `913194`: No existen contratos asociados para el número de identificación.
   - `801543`: El contrato ingresado es incorrecto, o no tiene contratos activos.
   - Cualquier otro código: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.
2. Si la lista viene vacía:
   - *No se encontraron contratos activos para el criterio de búsqueda.*
3. Si el contrato es válido:
   - Se guarda en memoria durante 86400 segundos.
   - Se ajusta la dirección corta a 71 caracteres si la tradicional supera 72.
   - Continúa a la confirmación de saldos TRB.

## Paso 3. Consulta por cédula o RUC

Pregunta:

> Por favor ingresa tu número de cédula o RUC.

Validación previa:
- Debe ser una identificación válida (10 dígitos para cédula ecuatoriana).

Servicio: **Get Kiosko TRB** (Tool ID `2896`)

Request:

```json
{
  "accion": "getKioskoTRBIdentificacion",
  "identificacion": "<cedula ingresada>",
  "idConsulta": "2"
}
```

Respuesta y Paginación:
- Valida los mismos códigos de error (`913194`, `801543`).
- Despliega la lista interactiva con los primeros 10 contratos encontrados vinculados a esa identificación.
- Al seleccionar el contrato deseado, se guarda en memoria y avanza a la confirmación.

## Salida

Al identificar un contrato válido, transfiere inmediatamente a:

- [_ 1.2 - Saldos TRB - Confirmación](./1-2-saldos-trb-confirmacion.md) (Skill ID `51076`).
