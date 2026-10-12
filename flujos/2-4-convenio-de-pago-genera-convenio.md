# 2.4 - Convenio de Pago - Genera Convenio

Formaliza y emite legalmente el convenio de pago en el sistema comercial de Interagua. Valida la disponibilidad de contacto telefónico, confirma las condiciones finales, obtiene parámetros de financiación y conceptos de deuda, ejecuta la creación del trámite, genera el cupón de pago con su fecha de vencimiento y entrega el código oficial al cliente.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato objeto del convenio.
- `identificacion`: Cédula o RUC del solicitante.
- `valorDeuda`: Total de deuda financiada.
- `cuotaInicial`: Valor de la cuota inicial acordada.
- `mesesFinanciar`: Plazo en meses pactado.
- `codigoProducto`, `codigoCliente`, `codigoDireccion`: Códigos maestros del contrato obtenidos en la simulación.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `user.phone`: Número de teléfono del cliente.

## Paso 1. Verificación de contacto telefónico

El sistema valida si el perfil del usuario cuenta con un número de teléfono registrado:
- **Si falta el teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para solicitarlo y registrarlo. Si el usuario no proporciona su número:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El trámite se cancela en este punto).*
- **Si el teléfono está presente:** Continúa al Paso 2.

## Paso 2. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- Si el servicio está en mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Si está operativo: Continúa al Paso 3.

## Paso 3. Confirmación final de aptitud para generación

Servicio: **Get Confirmacion Generacion Convenio** (Tool ID `2920`)  
Endpoint: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`  
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

Reglas de negocio:
- Si el contrato no permite la emisión (`APLICA_GENERACION_CONV !== 'S'`):  
  Mensaje: `Tu cuenta no cumple con las condiciones para realizar un convenio de pago, por favor comunícate con nosotros llamando al 134.`  
  Se registra marcador `ES Convenio Pago` y se transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`).
- Si el resultado es aprobatorio (`'S'`), continúa al Paso 4.

## Paso 4. Consulta de parámetros de financiación

Servicio: **Get Parametro Financiacion Generacion Convenio** (Tool ID `2921`)  
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

El servicio recupera y almacena en memoria:
- `idPlanAcuerdoPago`: Código del plan financiero (`PLAN_FINANCIACION`).
- `financiaDeudaVencida`: Indicador de financiamiento sobre vencidos (`FINA_DEUDA_VENCIDA`).

## Paso 5. Consulta de conceptos de deuda por contrato

Servicio: **Get Listado Conceptos Deuda Por Contrato** (Tool ID `2922`)  
Endpoint: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`  
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

El bot extrae el listado de conceptos (`CARGCONC`) y los almacena en el formato de lista requerido por el backend:
```json
{
  "string": ["CARGCONC_1", "CARGCONC_2"]
}
```
Guardado en memoria en `idConceptos` con un tiempo de vida (TTL) de 3600 segundos.

## Paso 6. Construcción de metadatos de auditoría

El sistema genera los datos de auditoría de la solicitud:
- `comentario`: Formateado como `WH <telefonoUsuario> Financiacion de deuda`.
- `fechaFinanciacion`: Fecha del día actual en zona horaria de Ecuador (`YYYY-MM-DD`).

## Paso 7. Emisión formal del convenio de pago

Servicio: **Generación Convenio** (Tool ID `3054`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "identificacion": "<identificacion>",
  "numeroContrato": "<numeroContrato>",
  "cuotaInicial": "<cuotaInicial>",
  "mesesFinanciar": "<mesesFinanciar>",
  "idPlanAcuerdoPago": "<idPlanAcuerdoPago>",
  "financiaDeudaVencida": "<financiaDeudaVencida>",
  "idConceptos": "<idConceptos>",
  "comentario": "<comentario>",
  "fechaFinanciacion": "<fechaFinanciacion>",
  "codigoCliente": "<codigoCliente>",
  "codigoDireccion": "<codigoDireccion>",
  "codigoProducto": "<codigoProducto>"
}
```

Diccionario de códigos de error:
- `901705`: `No hay deuda financiable.`
- `903357`: `Tienes un reclamo en proceso. Para continuar con la generación del convenio por favor comunicate al 134.`
- `919317`: `Su contrato no aplica con las condiciones para convenio de pago. Para mayor información contáctese al 134.`
- Si la respuesta está vacía: `Lo sentimos, hemos tenído un error al generar tu convenio de pago.`
- Cualquier otro código: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`

Si el servicio genera el convenio, se captura:
- `numeroSolicitud`: Código de solicitud asignado (`requestOut.requestId`).

## Paso 8. Obtención del cupón de pago y cálculo de vigencia

Servicio: **Get Formato Cupon Financiacion** (Tool ID `2924`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "numeroSolicitud": "<numeroSolicitud>"
}
```

El flujo procesa la fecha límite de pago devuelta (`FECHA_LIMITE`):
- Si la fecha es válida:
  > Este comprobante tiene validez hasta el DD/MM/YYYY, de no ser pagado en este plazo se anulará este convenio.
- Si no viene fecha o es inválida:
  > Este comprobante tiene validez hasta la fecha límite indicada en el convenio, de no ser pagado en este plazo se anulará.

Se guardan en memoria:
- `numeroCupon`: Número único de recaudación bancaria (`CUPON`).
- `texto`: Texto con la fecha límite de vencimiento.

## Paso 9. Entrega del comprobante al cliente

El bot entrega la confirmación definitiva:

> ¡Listo! El número de tu cupón generado es *{{$memory.numeroCupon}}*.  
>   
> Recuerda acercarte con este número en los puntos de pagos autorizados.   
>   
> {{$memory.texto}}

## Salida

- **Trámite completado exitosamente:** Registra el marcador analítico `F Convenio Pago` y transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`) para consultar si el usuario requiere otra gestión.
- **Fallas de negocio o servicio:** Registra `ES Convenio Pago` y transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
