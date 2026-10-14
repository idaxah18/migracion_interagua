# 1.2 - Saldos TRB - Inicio

Identifica el contrato TRB por número de contrato o por cédula/RUC y pasa a la confirmación del saldo.

## Datos de entrada

- Teléfono de la sesión.
- Consentimiento LOPD vigente.
- `contexto`: `saldos`, asignado en [1 - Saldos - Inicio](./1-saldos-inicio.md).
- `op_menu`: `saldo_trb`, elegido en [1 - Saldos - Menú](./1-saldos-menu.md).

Si se abre sin el menú: [0 - Inicio](./0-inicio.md).

## 1. Presentación

1. Marcador `Saldos TRB`.
2. Imagen: `https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10515%2Fimage%2Fa16453d6-944d-440b-84f1-1c9ca083ae38-1728423790790.jpeg`
3. Pausa de 3 segundos.

## 2. Modo de consulta

> ¿Quieres consultar tu saldo o pagos de TRB - “Tasa de Recolección de Basura” con tu número de contrato o número de cédula?

Botones:

- **Contrato** (`op_menu_saldo` = `contrato`)
- **Cédula o RUC** (`op_menu_saldo` = `identificacion`)

Si no elige una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

## 3. Por contrato

> Por favor ingresa tu número contrato TRB.

El valor escrito se guarda en `numeroContrato`. Debe coincidir con `^\d{1,8}$` (solo dígitos, 1 a 8).

Si no cumple:

> El número de contrato que ingresaste no es válido.
>
> _Recuerda que solo debes ingresar números hasta máximo 7 dígitos._

Se vuelve a pedir el número.

Servicio: [getKioskoTRB](../servicios/getKioskoTRB.md), búsqueda por contrato.

Se toma el primer registro y se guarda su `IDENTIFICACION` en `identificacion`. Luego se vuelve a consultar [getKioskoTRB](../servicios/getKioskoTRB.md) por esa identificación para armar la lista de contratos (paso 5).

## 4. Por cédula o RUC

> Por favor ingresa tu número de cédula o RUC.
>
> Recuerda digitar solo números, sin caracteres adicionales

El valor escrito se guarda en `identificacion`. Debe coincidir con `^(?:\d{10}|\d{13})$`.

Si no cumple:

> La identificación ingresada no tiene el formato correcto 📝
>
> Por favor, escribe únicamente los 10 números de tu cédula o 13 números de tu RUC, *sin puntos, guiones ni letras*.

Se vuelve a pedir la identificación.

Servicio: [getKioskoTRB](../servicios/getKioskoTRB.md), búsqueda por identificación.

## 5. Contratos encontrados

Con la lista que responde [getKioskoTRB](../servicios/getKioskoTRB.md):

- Si la dirección tradicional supera 72 caracteres, `DIRECCION_CORTA` queda en 71. Si no, se usa completa.
- Se guardan hasta 10 contratos en `Contratos` (86400 segundos). Cada ítem trae `ID_CONTRATO` y `DIRECCION_COMPLETA`.

| Condición | Mensaje (`msm_error`) |
| --------- | --------------------- |
| `errorCode` `913194` | No existen contratos asociados para el número de identificación. |
| `errorCode` `801543` | El contrato ingresado es incorrecto, o no tiene contratos activos. |
| Otro error | !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde. |
| Sin contratos | No se encontró contratos activos para el criterio de busqueda |
| Excepción | Hemos tenido un error en el servicio, por favor, inténtalo nuevamente más tarde |

Si hay error: [Mensaje de error personalizado](./mensaje-error-personalizado.md) o [Mensaje de error de servicio](./mensaje-error-servicio.md).

### 5.1 Vino por contrato

Se busca en `Contratos` el `ID_CONTRATO` igual a `numeroContrato`. Si está, ese ítem se guarda en `Contrato` (86400 segundos) y se pasa a confirmación. Si no está: [Mensaje de error de servicio](./mensaje-error-servicio.md).

### 5.2 Vino por identificación

Lista:

> A continuación te presentamos los contratos y direcciones asociados a tu identificación.
>
> ¿Sobre cuál quieres consultar el saldo de TRB?

Cada opción: título `{ID_CONTRATO}` y descripción `{DIRECCION_COMPLETA}` del contrato de la lista. Al elegirla se guarda `numeroContrato` y se busca ese contrato en `Contratos`. Si está, se guarda en `Contrato` y se pasa a confirmación. Si no: [Mensaje de error de servicio](./mensaje-error-servicio.md).

Si no elige una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

## Salida

Contrato válido: [1.2 - Saldos TRB - Confirmación](./1-2-saldos-trb-confirmacion.md).
