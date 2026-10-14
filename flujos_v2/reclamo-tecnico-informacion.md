# Reclamo Técnico - Información

Solicita la conformidad del usuario con el contrato y la dirección seleccionada, consulta los datos maestros de la cuenta mediante el servicio comercial de Interagua recuperando el código de producto, cliente y el identificador de dirección física (`ADDRESS_ID`), y deriva al flujo de registro del reclamo.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato seleccionado.
- `Contrato.DIRECCIONTRADICIONAL`: Dirección física registrada.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.

## Paso 2. Confirmación de contrato y dirección

El bot solicita la verificación de datos antes de consultar la cuenta:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:  
>   
> *Contrato No. {{$memory.numeroContrato}} - {{$memory.Contrato.DIRECCIONTRADICIONAL}}*  
>   
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Continúa a consultar los datos maestros del contrato (Paso 3).
2. **No:** Transfiere de regreso a [_ Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md).
3. **Salir:** Transfiere a [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md).

## Paso 3. Consulta de datos técnicos del contrato

Servicio: [getInfoContrato](../servicios/getInfoContrato.md).

Diccionario de códigos de error:
- `20082`: `La cédula ingresada es incorrecta.`
- `801543`: `El contrato ingresado es incorrecto, o no tiene contratos activos.`
- Si la respuesta no devuelve registros: `No se encontró información para el contrato ingresado`
- Si no posee dirección física (`ADDRESS_ID` vacío):  
  `Estimado cliente hemos verificado que al momento no consta con una dirección registrada con Interagua para el contrato consultado.`
- Fallback: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si la validación es satisfactoria:
- Se capturan las variables técnicas en memoria:
  - `codigoProducto`: Identificador del producto (`ID_PRODUCTO`).
  - `codigoCliente`: Identificador del cliente (`ID_CLIENTE`).
  - `codigoDireccion`: Identificador de la dirección física (`ADDRESS_ID`).
  - `codigoDireccionProducto`: Dirección del producto (`ADDRESS_ID`).
- Se guarda el objeto `infoContrato` con un tiempo de expiración (TTL) de 86400 segundos.

## Salida

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Registrar](./reclamo-tecnico-registrar.md).
- **Rechazo de datos:** [_ Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md).
- **Errores de validación:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
