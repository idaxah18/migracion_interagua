# Asistente con Inteligencia Artificial (Dev Gea)

Este flujo es un entorno de pruebas avanzadas y desarrollo de agentes de Inteligencia Artificial Generativa (motor Azure OpenAI GPT-4o) para la interacción en lenguaje natural de Interagua. Su propósito es experimentar con la extracción automática de intenciones del usuario, la validación inteligente de documentos de identidad, la verificación de políticas de privacidad (LOPD) y el reconocimiento de fotografías adjuntas.

## Datos que debe traer al entrar

- `{{$message.text}}`: Mensaje en lenguaje natural escrito por el usuario.
- `{{$user.phone}}`: Número de WhatsApp del cliente.
- Variables de sesión de prueba.

---

## Servicios y herramientas técnicas

### 1. Consulta de Política de Datos (LOPD)
- **Nombre de la herramienta:** queryPoliticaDatos
- **ID de la herramienta:** `1544`
- **Método HTTP:** `POST`
- **URL del servicio:** `https://chatbot.jelou.ai/v1/api/soap`
- **Función:** Comprueba en la tabla `1611` si el usuario ha autorizado el tratamiento de sus datos personales.

### 2. Consulta de contratos e identidad
- **Nombre de la herramienta:** Get Kiosko
- **ID de la herramienta:** `2936`
- **Método HTTP:** `POST`
- **URL del servicio:** `http://192.168.10.157/api-services-int/api/services` (Ambiente Desarrollo)
- **Función:** Recupera la información de la cuenta y el nombre del titular asociado a la cédula consultada.

---

## Paso 1. Notificación LOPD e intención del usuario

Si el usuario no cuenta con autorización registrada, el bot presenta el aviso de privacidad:

> Al continuar conectado usted autoriza el uso de sus datos personales a Interagua y sus empresas relacionadas. 
> 
> Usted puede consultar la política de privacidad en el siguiente link: bit.ly/PPInteragua

A continuación, un agente de IA (`gpt-4o-azure`) analiza el texto libre del cliente para determinar qué trámite desea realizar (consultar saldo, reportar fuga, convenios, etc.) y responde dinámicamente con la variable `IA_inicio`.

---

## Paso 2. Solicitud y validación de cédula de identidad

El bot solicita la identificación del usuario para individualizar su cuenta:

> Por favor, indíquenos su número de cédula de identidad:
> 
> _Recuerda digitar *solo números, sin caracteres adicionales*_

### Reglas de validación:
- Se evalúa mediante una expresión regular que contenga exactamente 10 dígitos numéricos (`^[0-9]{10}$`).
- **Si el formato es inválido:**
  El bot emite el mensaje de advertencia y solicita reingresar el dato:
  > La cédula ingresada no tiene el formato correcto 📝
  > 
  > Por favor, escribe únicamente los 10 números de tu cédula, *sin puntos, guiones ni letras*.

---

## Paso 3. Confirmación de identidad

Una vez validada la cédula contra el servicio `Get Kiosko`, el bot recupera el nombre del titular (`nombreCliente`) y presenta botones interactivos de confirmación:

> ¿Me puedes confirmar que tu nombre es {{$memory.nombreCliente}}?

Opciones:
1. **Sí** (Confirma que es el titular)
2. **No** (Solicita corregir el número o ingresar como tercero)

---

## Paso 4. Reconocimiento de imágenes adjuntas

El flujo incorpora un receptor de imágenes (`flow_foto`) que permite al usuario enviar fotografías de medidores, tuberías rotas o predios:

> Haz ingresado la siguiente foto {{$memory.flow_foto.images[0].file_url}}

La URL de la imagen se almacena en memoria para que el agente de IA o los inspectores técnicos puedan evaluar la novedad visualmente.

## Salidas del flujo

- **Transición a servicios:** Deriva al flujo de servicio correspondiente según la intención clasificada por la IA.
- **Salida de error o respaldo:** Si el servicio de consulta no responde, deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`) o a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
