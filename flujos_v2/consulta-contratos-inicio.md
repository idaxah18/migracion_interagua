# Consulta de contrato

Identifica el contrato del cliente y, si la consulta es válida, continúa el trámite que abrió este flujo.

El flujo no fija el texto del menú ni el de la lista. Esos dos textos llegan escritos. El resto de los mensajes de esta página sí están fijos.

## Datos que debe traer al entrar

- Texto del menú inicial.
- Texto de la lista de contratos.
- Dirección del servicio: [http://192.168.10.157/api-services-int/api/services](http://192.168.10.157/api-services-int/api/services)
- Ambiente: Producción.
- Contexto: opción por la que llega

## Mensajes que llegan escritos

El menú inicial muestra el primer texto. La lista de contratos muestra el segundo.

Saldo de consumo de agua:

- ¿Quieres consultar tu saldo de consumo de agua con tu número de contrato o número de cédula?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres consultar tu saldo de consumo de agua?

Convenio de pago:

- ¿Quieres realizar un convenio de pago con tu número de cédula o con tu número de contrato?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres realizar tu convenio de pago?

Alta facturación:

- ¿Quieres registrar tu reclamo con tu número de cédula o con número de contrato?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres registrar tu reclamo?

Reclamo técnico:

- ¿Quieres registrar tu reclamo con tu número de cédula o con número de contrato?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres realizar tu reclamo?

Afectación del servicio:

- ¿Deseas consultar si existe una afectación del servicio con tu número de cédula o con número de contrato?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres consultar si existe una afectación del servicio?

Venta de servicios y certificados:

- ¿Quieres continuar con tu número de contrato o número de cédula?
- A continuación te presentamos los contratos y direcciones asociados a tu identificación.
  ¿Sobre cuál quieres continuar?

## Paso 1. Elegir cómo consultar

Mostrar el texto del menú inicial con dos opciones:

1. Contrato
2. Cédula o RUC

Contrato guarda el modo `contrato` y pide el número de contrato.

Cédula o RUC guarda el modo `identificacion` y pide la cédula o el RUC.

La elección es obligatoria. Si la persona escribe algo que no es una opción:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

## Paso 2. Consulta por número de contrato

Pregunta:

> Por favor ingresa tu número de contrato.

Servicio: [getInfoContrato](../servicios/getInfoContrato.md). Se envía el número ingresado como contrato y como identificación.

Si el servicio responde, se toma el primer contrato de la lista y se valida en este orden:

1. Si la respuesta trae error, se usa el código:
  - `20082`: La cédula ingresada es incorrecta.
  - `801543`: El contrato ingresado es incorrecto, o no tiene contratos activos.
  - Cualquier otro código: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.
2. Si no viene ningún contrato: No se encontró información para el contrato ingresado.
3. Si el contrato no tiene `ADDRESS_ID`: Estimado cliente hemos verificado que al momento no consta con una dirección registrada con Interagua para el contrato consultado.
4. Si la validación se cae: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

Esos cuatro casos muestran el texto correspondiente y el flujo se detiene.

Si el contrato es válido:

- Se guarda el contrato durante 86000 segundos.
- La dirección corta queda en 71 caracteres cuando la dirección tradicional supera 72. Si no la supera, se usa completa.
- La identificación del trámite pasa a ser `IDENTIFICACION_BENEF`.
- Se continúa según el trámite. Ver la sección de salida.

Si la llamada al servicio falla, o esta validación no puede ejecutarse:

> ¡Lo sentimos!, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

Ese aviso usa signos de exclamación de apertura. Los textos de validación usan `!Lo sentimos¡`.

## Paso 3. Consulta por cédula o RUC

Pregunta:

> Por favor ingresa tu número de cédula o RUC.

Servicio: [getKioskoInteragua](../servicios/getKioskoInteragua.md). La búsqueda es por identificación.

Si el servicio responde, se valida así:

1. Si la respuesta trae error, se usa el código:
  - `913194`: No existen contratos asociados para el número de identificación.
  - `801543`: El contrato ingresado es incorrecto, o no tiene contratos activos.
  - Cualquier otro código: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.
2. Si la lista viene vacía: No se encontraron contratos activos para el criterio de búsqueda.
3. Si la validación se cae: !Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

Esos casos muestran el texto y el flujo se detiene.

Si hay contratos, a cada uno se le calcula la dirección corta con la misma regla de 71 caracteres.

Si el modo es `identificacion`, se muestra la lista.

Si la llamada falla, o esta validación no puede ejecutarse, se muestra:

> ¡Lo sentimos!, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

## Paso 4. Lista de contratos

Texto: el de la lista que llegó escrito.

Cada opción muestra el número de contrato (`ID_CONTRATO`).

Paginación:

- Con 10 contratos o menos, una sola lista con todos. No aparece "Siguientes Contratos".
- Con más de 10, páginas de 9. El corte avanza de 9 en 9 mientras el inicio de la página sea menor o igual a la cantidad total. Por eso la última página puede salir vacía. Ejemplo: 18 contratos arman tres páginas, y la tercera no tiene contratos.
- En la última página no aparece "Siguientes Contratos".
- En las páginas anteriores sí aparece, como última opción: Siguientes Contratos.
- Al elegirla, se pasa a la página siguiente y se vuelve a mostrar la lista.

Al elegir un número de contrato:

1. Se guarda ese número.
2. Se busca en la lista completa, no solo en la página visible.
3. Si está, se guarda ese contrato y se continúa según el trámite.
4. Si no está, se muestra: ¡Lo sentimos!, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.

Hay que elegir una opción de la lista. Si escribe cualquier cosa que no sea una opción de la lista, se debe enviar un texto que no coincida muestra:

> Debes seleccionar una opción para continuar. Escribe *Salir* para cancelar.

## Salida

Con un contrato válido, el trámite decide el siguiente paso:

La confirmación del reclamo técnico usa este texto:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:
>
> *Contrato No. {número} - {dirección tradicional}*
>
> ¿Estás de acuerdo con la información ingresada?

Botones: Sí, No
