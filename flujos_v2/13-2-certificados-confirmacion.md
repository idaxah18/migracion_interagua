# 13.2 - Certificados - Confirmación

Permite al cliente elegir el tipo de certificado requerido (Certificado de No Adeudar o Certificado de Suscripción del Servicio), valida la inexistencia de deudas pendientes en la cuenta, formaliza la emisión en el sistema comercial de Interagua y notifica que el documento oficial será remitido al correo electrónico registrado.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato.
- `identificacion`: Identificación del solicitante.
- `datos_cliente`: Información del beneficiario obtenida mediante `Get Datos Cliente` (`NOMBRE_BENEFICIARIO`, `DIRECCIONTRADICIONAL`, `IDENTIFICACION_BENEF`).
- `correo_selected`: Correo electrónico del cliente.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 2. Selección del tipo de certificado

El bot presenta los tipos de certificados disponibles:

> ¿Qué tipo de certificado necesitas?

Opciones con botones:
1. **No Adeudar:** Procede a la emisión del Certificado de No Adeudar (Paso 3A).
2. **Suscripción Servicio:** Procede a la emisión del Certificado de Suscripción (Paso 3B).

## Paso 3A. Emisión de Certificado de No Adeudar

Servicio: [registerSolicitudCertificado](../servicios/registerSolicitudCertificado.md).

Evaluación de excepciones:
- **Código `41` (Deuda pendiente con la empresa):**  
  > Para solicitar este servicio Ud debe regularizar su deuda con la Empresa en la opción "Pagos en Línea", "Convenio de Pago" o puede comunicarse con nosotros al 134.  
  *(Ofrece botón hacia [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md)).*
- **Código `1` (Solicitud preexistente registrada):**  
  > Ya cuentas con la solicitud {{$memory.numeroContrato}} en estado registrado, pronto atenderemos tu requerimiento.
- **Código `20201`:** Permite cancelar los valores mediante [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).

## Paso 3B. Emisión de Certificado de Suscripción

Servicio: [registroSolicitudSuscripcion](../servicios/registroSolicitudSuscripcion.md).

## Paso 4. Entrega de confirmación de despacho

Al emitirse satisfactoriamente el documento:

> Se ha generado tu solicitud *# {{$memory.response_certificado.data.PACKAGE_ID}}*, su certificado será enviado a su correo electrónico 

## Salida

- **Trámite culminado:** Transfiere a [_ Atención Adicional](./atencion-adicional.md).
- **Deuda pendiente:** Ofrece acceso directo a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
