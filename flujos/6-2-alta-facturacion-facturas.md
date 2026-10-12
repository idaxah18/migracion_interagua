# 6.2 - Alta Facturación - Facturas

Verifica la conformidad del contrato y la dirección, consulta los datos maestros de la cuenta, extrae el listado de facturas emitidas elegibles para impugnación, permite al cliente seleccionar una o varias planillas consultando sus cargos técnicos específicos, y solicita su confirmación final antes del registro formal del reclamo.

## Datos que debe traer al entrar

- `numeroContrato`: Código del contrato seleccionado.
- `Contrato.DIRECCION_COMPLETA`: Dirección registrada del predio.
- `MONTH_NAMES`: Nombres de los meses del año para presentación amigable.
- `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `contexto`: `alta_facturacion`.

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

El bot solicita la confirmación de los datos antes de consultar el sistema comercial:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:  
>   
> *Contrato No. {{$memory.numeroContrato}} - {{$memory.Contrato.DIRECCION_COMPLETA}}*  
>   
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Continúa a consultar los datos maestros del contrato (Paso 3).
2. **No:** Transfiere de regreso a [_ 6.1 - Alta Facturación - Contratos](./6-1-alta-facturacion-contratos.md) (Skill ID `51090`) para seleccionar otro contrato.

## Paso 3. Consulta de datos maestros del contrato para reclamo

Servicio: **Get Info Registro Reclamo** (Tool ID `2960`)  
Endpoint: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "numeroContrato": "<numeroContrato>"
}
```

Diccionario de códigos de error:
- `111112`, `801543`, `3303`: `El contrato ingresado es incorrecto, o no tiene contratos activos.`
- Sin registros devueltos: `No se encontró información para el contrato ingresado`
- Fallback: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si la validación es satisfactoria, se guardan en memoria durante 86000 segundos:
- `codigoDireccionContrato`: Código de dirección (`ID_DIRECCION_CONTRATO`).
- `codigoCliente`: Identificador del cliente (`ID_CLIENTE`).
- `codigoAplicante`: Código aplicante (`ID_CLIENTE`).
- `Factura`: Arreglo vacío inicial.

## Paso 4. Consulta del historial de facturas elegibles

Servicio: **Get Detalle Factura Reclamo** (Tool ID `2961`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "numeroContrato": "<numeroContrato>"
}
```

Reglas de procesamiento de facturas:
1. Si trae error `111112`, `801543` o `3303`: `El contrato ingresado es incorrecto, o no tiene contratos activos.`
2. Si la lista está vacía: `No se encontró facturas para el contrato ingresado`
3. Fallback: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si el servicio devuelve planillas:
- A cada factura se le asocia su día (`FECHA_VENCIMIENTO.split('/')[0]`) y el nombre de mes (`MONTH_NAMES[MES - 1]`).
- Se ordenan cronológicamente de la más reciente a la más antigua (`ANIO`, `MES`, `DIA`).
- Se almacena el arreglo `Facturas` en memoria con una vigencia de 86000 segundos.

## Paso 5. Selección interactiva de facturas y desglose de cargos

El bot presenta la lista interactiva con las facturas disponibles:

> {{$context.msm_facturas}}

Al pulsar una factura (`idFactura`), el bot consulta sus conceptos detallados de cobro:

Servicio: **Get Detalle Cargos** (Tool ID `2981`)  
Endpoint: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "codigoFactura": "<idFactura>"
}
```

Validación de cargos:
- Código `9061`: `El número de solicitud no puede ser nulo.`
- Código `900431`: `El código de la factura es inválido.`
- Si no hay cargos: `No se encontró detalles de cargos para la factura consultada`

Si es exitoso, mapea cada cargo a su formato técnico:
- `codigoProducto`: `ProductId`
- `codigoSegmentoCobro`: `AccountId`
- `codigoConcepto`: `ConceptId`
- `signo`: `Sign`
- `unidades`: `ConceptUnid`
- `valor`: `ConceptValue`
- `codigoCausalCargo`: `1`

Se marca la factura en memoria como `SELECTED: true` y se le asigna su lista de `Cargos`.

## Paso 6. Opción para agregar facturas adicionales

El bot pregunta con botones si desea incluir más planillas en el mismo reclamo:

> Ok, ¿Deseas seleccionar otra factura?

Opciones con botones:
1. **Sí:** Regresa a la lista excluyendo las facturas ya agregadas para sumar otra.
2. **No:** Continúa a la confirmación final (Paso 7).

## Paso 7. Confirmación del resumen de facturas

El bot presenta el consolidado de las facturas que conformarán el reclamo:

> ¡Perfecto! Estas son las facturas seleccionadas para tu reclamo.  
>   
> {{$context.msm_confirmacion}}  
>   
> ¿Deseas confirmar el registro?

Opciones con botones:
1. **Confirmar:** Estructura la variable `listaFactura` y transfiere a [_ 6.3 - Alta Facturación - Registrar](./6-3-alta-facturacion-registrar.md) (Skill ID `51092`).
2. **Modificar selección:** Reinicia el ciclo para ajustar las planillas elegidas.

## Salida

- **Confirmación aprobada:** [_ 6.3 - Alta Facturación - Registrar](./6-3-alta-facturacion-registrar.md) (Skill ID `51092`).
- **Rechazo de datos de contrato:** [_ 6.1 - Alta Facturación - Contratos](./6-1-alta-facturacion-contratos.md) (Skill ID `51090`).
- **Fallas de servicio:** Registra `ES Alta Fact` y transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
