# Reclamo Técnico - Mensaje Personalizado

Interpreta las excepciones técnicas y comerciales devueltas por los sistemas de Interagua durante la creación de un reclamo técnico o daño a producto. Si la causa es una solicitud preexistente, consulta el expediente previo en el Web Service para informar el código de trámite y su estado; en los demás casos, traduce la condición comercial en una explicación comprensible para el cliente.

## Datos que debe traer al entrar

- `codigoError`: Código devuelto por el servicio `Register Dano Producto` o `Register Incidente`.
- `DescripcionError`: Mensaje descriptivo de error devuelto por el backend.
- `numeroContrato`: Código de contrato.
- `identificacion`: Identificación del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.

## Paso 2. Clasificación de la excepción comercial

El bot analiza la combinación de `codigoError` y `DescripcionError`:

### 1. Reclamos preexistentes en curso (`3450` o `9564` con solicitud pendiente)
Si existe una solicitud previa registrada para la misma causal o un reclamo técnico activo de alcantarillado, el bot consulta el trámite en el Paso 3.

### 2. Contrato sin servicio de alcantarillado activo (`2741` o `9564`)
Condición: `El contrato no cuenta con el servicio de alcantarillado activo.`  
Mensaje:
> Estimado cliente, hemos verificado que el contrato consultado no cuenta con el servicio de alcantarillado activo en nuestros registros comerciales.

### 3. Servicio suspendido (`9564`)
Condición: `El contrato refleja suspensión del servicio.`  
Mensaje:
> Estimado cliente, el contrato refleja suspensión del servicio. Por favor comunícate con nosotros llamando al 134 para mayor información.

### 4. Reposición de medidor en trámite (`2741`)
Condición: `Existe una solicitud por reposición de medidor pendiente de atención.`  
Mensaje:
> Estimado cliente, existe una solicitud por reposición de medidor pendiente de atención para este contrato.

### 5. Otras excepciones (`919446`, `902543` o no catalogadas)
El flujo utiliza el texto de `DescripcionError` o el mensaje por defecto:
> Estimado cliente no hemos podido registrar tu reclamo. Para mayor información contáctate al 134.

## Paso 3. Consulta de solicitud en curso (para solicitudes pendientes)

Servicio: [getSolicitudPorContrato](../servicios/getSolicitudPorContrato.md).

Reglas de recuperación:
- Recupera la solicitud más reciente registrada o en atención.
- Construye el mensaje con el número de paquete y estado:
  > Estimado, existe un reclamo *{{$memory.SolicitudRegistrada.PACKAGE_ID}}* en estado {{$memory.SolicitudRegistrada.DESC_EST_TRAMITE}} por el mismo concepto.  
  > Para mayor información puedes ingresar a la opción "Consulta de Trámites" o comunicarte al 134.

## Salida

- **Despliegue del mensaje:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) asignando `msm_error`.
- **Fallas técnicas de consulta:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
