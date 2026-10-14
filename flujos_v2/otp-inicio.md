# Validación de código de seguridad (OTP)

Verifica la identidad del cliente antes de trámites con costo o compromiso comercial, como la venta de servicios o los certificados administrativos. Consulta los correos del contrato, deja elegir uno enmascarado, envía un código de un solo uso y lo valida con un máximo de 3 intentos.

## Datos de entrada

- `identificacion`: cédula o RUC del cliente.
- `numeroContrato`: contrato del trámite.
- `contexto`: trámite que pide la verificación (`venta_servicios`, `certificados_administrativos` u otro).
- `is_trampa_grasa`: indica si el trámite es de trampa de grasa.

## 1. Correos del contrato

Servicio: [getListadoCorreoCliente](../servicios/getListadoCorreoCliente.md).

Si no hay correos:

> No se encontró un correo electrónico asociado a tu contrato.

Se transfiere a [Ayuda personalizada](./ayuda-personalizada.md).

## 2. Selección del correo

Los correos se muestran enmascarados (primeros 4 caracteres del usuario y últimos 6 del dominio, por ejemplo `nels****@****.com`) junto con la opción de un asesor:

> Seleccione su correo electrónico que se encuentra en nuestros sistemas, si no lo visualiza seleccione Chatear con un asesor:
>
> 1 - user****@****.com
> 2 - Chatear con un asesor
>
> *Ingresa 0 para salir del menú*

- **0:** [Atención adicional](./atencion-adicional.md).
- **Chatear con un asesor:** [Ayuda personalizada](./ayuda-personalizada.md).
- **Un correo:** se guarda en `correo_selected` y se envía el código.

## 3. Envío del código

Servicio: [generarOtp](../servicios/generarOtp.md).

Si el envío falla, se muestra [Mensaje de error de servicio](./mensaje-error-servicio.md).

Si el envío es correcto, se inician 3 intentos (`num_intentos = 3`) y se pide el código de 6 dígitos.

## 4. Validación del código

El cliente escribe el código. Servicio: [validarOtp](../servicios/validarOtp.md).

- **Código válido:** [OTP - Derivación](./otp-deriva-contexto.md).
- **Código incorrecto:** se resta un intento.
  - Si quedan intentos, se pide el código otra vez.
  - Si se agotan los 3 intentos:
    > ¡Ups! Parece que hubo un inconveniente al validar tu código de seguridad
    Se transfiere a [0 - Inicio](./0-inicio.md).
