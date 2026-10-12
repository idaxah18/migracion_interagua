# 2.2 - Convenio de Pago - Porcentaje y Plazo

Confirma con el cliente los datos del contrato y la dirección elegida, consulta si el contrato mantiene deudas financiables y cumple las condiciones comerciales para simulación virtual, y le permite elegir el porcentaje de cuota inicial y el plazo de financiamiento en meses mediante listas interactivas.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato seleccionado.
- `Contrato.DIRECCIONTRADICIONAL`: Dirección registrada del inmueble.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `contexto`: `convenio_pago`.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- Si el servicio está en mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Si está operativo: Continúa al Paso 2.

## Paso 2. Confirmación de contrato y dirección

El bot solicita la conformidad del cliente antes de consultar el estado de la deuda:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:  
>  
> *Contrato No. {{$memory.numeroContrato}} - {{$memory.Contrato.DIRECCIONTRADICIONAL}}*  
>  
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Continúa a la consulta de deuda (Paso 3).
2. **No:** Transfiere de regreso a [_ 2.1 - Convenio de Pago - Contratos](./2-1-convenio-de-pago-contratos.md) (Skill ID `51099`) para seleccionar otro contrato.

## Paso 3. Consulta de deuda y elegibilidad para convenio

Servicio: **Get Info Simulacion Convenio** (Tool ID `2905`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "identificacion": "<identificacion>",
  "numeroContrato": "<numeroContrato>"
}
```

El flujo procesa la respuesta bajo las siguientes reglas comerciales:

1. **Errores del servicio:**
   - Código `3303`: Debe ingresar la información requerida por el contrato.
   - Cualquier otro código: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`
2. **Sin información:** Si el contrato no retorna registros:
   - Mensaje: `No se encontró información para el contrato ingresado`
3. **Sin deuda vencida:** Si `DEUDA_PENDIENTE <= 0`:
   - Mensaje: `Estimado cliente hemos verificado que al momento no tienes una deuda con Interagua.`
4. **No elegible para canal virtual:** Si `APLICA_SIMULACION_VIRTUAL !== 'S'`:
   - Mensaje: `Tu cuenta no cumple con las condiciones para realizar un convenio de pago, por favor comunícate con nosotros llamando al 134.`

Si se presenta cualquiera de estos casos, el sistema registra el marcador analítico `ES Convenio Pago` y deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) mostrando el mensaje respectivo.

Si la cuenta es apta, se guardan las variables en memoria con una vigencia de 3600 segundos:
- `valorDeuda`: Saldo adeudado (`DEUDA_PENDIENTE`).
- `codigoProducto`: Identificador del producto (`ID_PRODUCTO`).
- `codigoCliente`: Identificador del cliente (`ID_CLIENTE`).
- `codigoDireccion`: Identificador de dirección (`ID_DIRECCION_CONTRATO`).

## Paso 4. Elección del porcentaje de cuota inicial

Servicio: **Get Listado Porcentaje Cuota Inicial** (Tool ID `2914`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "identificacion": "<identificacion>",
  "valorDeudaPendiente": "<valorDeuda>"
}
```

Si la consulta falla o no retorna opciones:
- Mensaje: `No se encontró información de porcentajes de cuota inicial.`  
- Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md).

Si retorna porcentajes válidos, el bot presenta una lista interactiva:

> Ok, te comentamos que tu deuda actual es de ${{$memory.valorDeuda}}  
>  
> Elige el porcentaje de tu cuota inicial.

Opciones dinámicas de la lista:
- `{{$item.PORCENTAJE}}%` (ej. 10%, 20%, 30%, etc.)

Al elegir una opción, se guarda en memoria la variable `porcentaje`.

## Paso 5. Elección del plazo de financiamiento (meses)

Servicio: **Get Listado Meses de Financiacion** (Tool ID `2915`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "identificacion": "<identificacion>",
  "valorDeudaPendiente": "<valorDeuda>"
}
```

Si el servicio no devuelve meses válidos:
- Mensaje: `No se encontró información para el contrato ingresado`  
- Deriva a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md).

Si devuelve opciones de financiamiento, presenta la lista interactiva:

> A cuantos meses deseas financiar tu convenio.

Opciones dinámicas de la lista:
- Botones postback con los plazos habilitados (ej. 3 meses, 6 meses, 12 meses).

Al seleccionar el plazo, se guarda la variable `mesesFinanciar`.

## Salida

- **Continuación normal:** Transfiere a [_ 2.3 - Convenio de Pago - Simulación](./2-3-convenio-de-pago-simulacion.md) (Skill ID `51095`) para calcular las cuotas exactas y mostrar los términos y condiciones.
- **Rechazo de datos de contrato:** Transfiere de regreso a [_ 2.1 - Convenio de Pago - Contratos](./2-1-convenio-de-pago-contratos.md) (Skill ID `51099`).
- **Fallas o no elegibilidad:** Registra marcador `ES Convenio Pago` y transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
