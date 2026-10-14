# 6.4 - Alta Facturación - Mensaje Personalizado

Interpreta y traduce los códigos de excepción comercial devueltos por el sistema de Interagua durante la creación de un reclamo por facturación. Si existe un reclamo duplicado, consulta el sistema para recuperar el número de trámite activo y su estado actual, entregando una explicación clara y guiada al usuario.

## Datos que debe traer al entrar

- `codigoError`: Código técnico devuelto por el servicio `Registro Reclamos Facturación`.
- `DescripcionError`: Descripción del error devuelta por el sistema comercial.
- `numeroContrato`: Código de contrato.
- `identificacion`: Identificación del solicitante.
- `descTipoCausal`: `503 - ALTA FACTURACIÓN`.
- `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`.

## Paso 2. Clasificación del código de excepción

El bot clasifica el error y asigna la respuesta al cliente según la causa:

### Casos directos:
1. **Código `903501` (Inconsistencia de contrato):**
   > La(s) factura(s) ingresada no pertenece al contrato ingresado.
2. **Código `904283` (Reclamo en trámite):**
   > Ya tienes un reclamo en curso por la(s) factura(s) seleccionada(s), para mayor información te recomiendo llamar al 134.
3. **Código `2000` (Facturas con financiamiento):**
   > Estimado, la(s) factura(s) seleccionada(s) están dentro de un convenio de pago vigente.  
   > Por favor comunícate al 134 para mayor información.

### Casos de duplicidad por el mismo periodo (`903504` o `2741`):
Si el código devuelto es `903504` o `2741`, el bot no muestra un error genérico, sino que procede al Paso 3 para consultar el expediente exacto.

## Paso 3. Consulta del reclamo previo en curso

Servicio: [getSolicitudPorContrato](../servicios/getSolicitudPorContrato.md).

Reglas de búsqueda:
- Busca la solicitud donde:
  - `DESC_TIPO_CAUSAL_MOTIVO` coincida con `503 - ALTA FACTURACIÓN`.
  - El estado (`DESC_EST_MOTIVO`) sea `13 - Registrada` o `14 - Atendida`.
- Si se encuentra la solicitud coincidente, se compone el mensaje:
  > Estimado, existe un reclamo *{{$memory.SolicitudRegistrada.PACKAGE_ID}}* en estado {{$memory.SolicitudRegistrada.DESC_EST_TRAMITE}} por el mismo periodo.  
  > Para mayor información puedes ingresar a la opción "Consulta de Trámites" o comunicarte al 134.
- Si no se encuentra información o falla la consulta, muestra el mensaje de reclamo en curso estándar.

## Paso 4. Registro analítico y derivación de mensaje

Una vez asignado el texto en `msm_error`:
1. Se registra el marcador analítico: `EP Alta Fact`.
2. Se transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) para presentar el mensaje en pantalla.

## Salida

- **Despliegue del mensaje:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md).
- **En caso de error en consulta:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
