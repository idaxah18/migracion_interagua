# Validación de código de seguridad (OTP) - Inicio

Este flujo implementa el protocolo de autenticación reforzada y verificación de identidad en dos pasos mediante contraseña de un solo uso (OTP) por correo electrónico. Se activa antes de permitir transacciones de alta seguridad o que generen compromisos comerciales y legales (como la contratación de venta de servicios de ingeniería, certificados de trampa de grasa o emisión de certificados administrativos con costo).

El sistema consulta los correos registrados para la cuenta, enmascara las direcciones para proteger la privacidad del titular, despacha el código numérico seguro y valida su concordancia con un límite estricto de intentos.

## Datos que debe traer al entrar

- `identificacion`: Cédula o RUC del cliente.
- `numeroContrato`: Número de contrato asociado al trámite.
- `contexto`: Identificador del trámite que solicita la validación de identidad (`venta_servicios`, `certificados_administrativos`, etc.).
- `is_trampa_grasa`: Indicador booleano que determina si el trámite corresponde a una trampa de grasa.

---

## Servicio 1: Obtener listado de correos del cliente

El sistema consulta las direcciones de correo electrónico registradas y autorizadas en la ficha del contrato.

- **Nombre de la herramienta:** Get Listado Correo Cliente
- **ID de la herramienta:** `6419`
- **Método HTTP:** `POST`
- **URL del servicio:** `{{$context.url}}` (Servicio interno de clientes)
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "accion": "getListadoCorreoCliente",
    "identificacion": "<identificacion>",
    "numeroContrato": "<numeroContrato>"
  }
  ```
- **Respuesta esperada:** Arreglo con la lista de correos electrónicos (`CORREO_ELECTRONICO`) vinculados al contrato.

### Si no se encuentran correos asociados:
El bot informa al usuario:
> No se encontró un correo electrónico asociado a tu contrato.
Y transfiere al cliente a [Ayuda Personalizada](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/ayuda-personalizada.md) (Skill `51108`) para que un asesor actualice sus datos de contacto.

---

## Paso 1. Selección de correo electrónico enmascarado

Cuando se recuperan correos válidos, el sistema enmascara la información sensible (por ejemplo, mostrando únicamente los primeros 4 caracteres del usuario y los últimos 6 del dominio: `nels****@****.com`), añade una opción para conectar con un asesor y presenta un menú interactivo:

> Seleccione su correo electrónico que se encuentra en nuestros sistemas, si no lo visualiza seleccione Chatear con un asesor:
> 
> 1 - user****@****.com  
> 2 - Chatear con un asesor  
> 
> *Ingresa 0 para salir del menú*

### Respuestas posibles:
- **Digitar `0`:** El usuario cancela la gestión y pasa a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).
- **Seleccionar "Chatear con un asesor":** Se deriva a la mesa de ayuda humana para actualizar los datos del contrato.
- **Seleccionar un número de correo:** Se guarda la dirección seleccionada en `correo_selected` y se procede al envío del código.

---

## Servicio 2: Generación y despacho del código OTP

El sistema solicita al servidor central de seguridad la generación de un código temporal de un solo uso y su envío por correo electrónico.

- **Nombre de la herramienta:** Generar OTP
- **ID de la herramienta:** `6436`
- **Método HTTP:** `POST`
- **URL del servicio:** `https://chatbot.jelou.ai/v1/api/soap` (Proxy SOAP Jelou)
- **Función SOAP:** `GenerarOTP`
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "url": "{{$context.url}}",
    "functionName": "GenerarOTP",
    "data": {
      "identificacion": "<identificacion>",
      "correoUsuario": "<correo_selected>",
      "contrato": "<numeroContrato>"
    }
  }
  ```
- **Respuesta esperada:** Código de estado satisfactorio confirmando el envío del correo.

Una vez despachado el código, el sistema inicializa el contador de intentos (`num_intentos = 3`) y le pide al cliente que revise su bandeja de entrada y escriba el código de 6 dígitos.

---

## Servicio 3: Validación del código ingresado

El usuario digita el código de seguridad recibido y el sistema lo valida en tiempo real.

- **Nombre de la herramienta:** Validar OTP
- **ID de la herramienta:** `6437`
- **Método HTTP:** `POST`
- **URL del servicio:** `https://chatbot.jelou.ai/v1/api/soap` (Proxy SOAP Jelou)
- **Función SOAP:** `ValidarOTP`
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "url": "{{$context.url}}",
    "functionName": "ValidarOTP",
    "data": {
      "identificacion": "<identificacion>",
      "correoUsuario": "<correo_selected>",
      "contrato": "<numeroContrato>",
      "codigoOtp": "<codigo_otp>"
    }
  }
  ```

### Control de intentos y resultados de la validación:
- **Código válido y verificado:**
  La identidad queda confirmada y el usuario avanza de inmediato a [OTP - Deriva Contexto](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/otp-deriva-contexto.md) (Skill `51083`).
- **Código incorrecto:**
  El sistema resta un intento (`num_intentos = num_intentos - 1`):
  - **Si aún quedan intentos disponibles (`num_intentos > 0`):**
    El bot notifica que el código no coincide y le pide ingresarlo nuevamente.
  - **Si se agotan los 3 intentos (`num_intentos <= 0`):**
    El bot bloquea el proceso por seguridad con el mensaje:
    > ¡Ups! Parece que hubo un inconveniente al validar tu código de seguridad
    Y deriva al usuario a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
