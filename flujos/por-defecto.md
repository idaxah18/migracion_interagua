# Mensaje por defecto (Fallback global)

Este flujo se activa automáticamente cada vez que el cliente escribe un mensaje de texto libre o una palabra que el bot no logra reconocer dentro del contexto de la conversación actual. Su función principal es reconducir al usuario con empatía hacia las opciones válidas del menú principal, evitando que la conversación se interrumpa, y ofreciendo asistencia humana si el usuario experimenta dificultades reiteradas.

## Datos que debe traer al entrar

- `{{$message.text}}`: Mensaje de texto original escrito por el usuario en WhatsApp.
- `num_intentos_ne`: Contador en memoria de intentos no entendidos durante la sesión activa.

## Paso 1. Comprobación de contacto telefónico

Antes de responder, el sistema ejecuta la verificación de teléfono mediante el componente [Guardia de teléfono](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/guardia-telefono.md) (Skill `51038`):

- **Si el usuario no comparte su contacto:**
  El bot finaliza amablemente el intento con el mensaje:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.

- **Si el contacto es confirmado:**
  El flujo captura el mensaje en la variable `first_message` y evalúa sus características.

## Paso 2. Evaluación de intentos y longitud del mensaje

El sistema ejecuta una regla de validación de texto:
1. Comprueba si la longitud del texto es menor a 100 caracteres (`is_valid_message`).
2. Incrementa el contador de intentos no entendidos: `num_intentos_ne = num_intentos_ne + 1`.

A continuación, evalúa el número de fallos acumulados:

### Caso A: 3 o más intentos consecutivos no entendidos (Ofrecimiento de asesor)
Si el usuario ha escrito 3 o más veces sin que el bot logre comprender su requerimiento, el sistema presume que necesita ayuda humana y le presenta un mensaje interactivo con botones:

> Parece que estás teniendo dificultades para registrar tu requerimiento.
> 
> ¿Deseas que te comunique con un asesor ?

Opciones:
1. **Sí**
2. **No**

Si el usuario responde **Sí**:
1. **Solicitud de nombre:** El bot pregunta su nombre completo y lo guarda en `nombres_asesoria`.
2. **Detalle de la consulta:** Pregunta el motivo de su requerimiento y lo guarda en `requerimiento_asesoria`.
3. **Conexión:** Intenta transferir la sesión a un operador humano disponible:
   - **Conexión exitosa:**
     > Por favor permíteme validar la información que nos proporcionaste 🙂
   - **No hay operadores disponibles:** Deriva a [Operator Not Found](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-not-found.md) (Skill `51040`).
   - **Fuera del horario de atención:** Deriva a [Operator Out Schedule](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-out-schedule.md) (Skill `51041`).

---

### Caso B: Menos de 3 intentos no entendidos (Reconducción al Menú)
Si el contador es inferior a 3, el sistema selecciona de forma aleatoria un mensaje empático y cercano para no sonar repetitivo, asignándolo a la variable `msm_menu_principal`:

- **Variante 1:**
  > 😄 Ups, esta vez no entendí lo que me dijiste. Pero aquí tienes lo que sí puedo hacer por ti:

- **Variante 2:**
  > 😅 Ups, todavía estoy aprendiendo y no entendí lo que me dijiste. Pero mira, sí puedo ayudarte con esto:

Inmediatamente después de fijar el mensaje, deriva al usuario al flujo [0.4 - Menú Principal - Opciones](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-4-menu-principal-opciones.md) (Skill `51039`) para desplegar la lista de trámites disponibles de Interagua.

## Salidas del flujo

- **Reorientación al autoservicio:** Transfiere a [0.4 - Menú Principal - Opciones](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-4-menu-principal-opciones.md) (Skill `51039`).
- **Transferencia a soporte humano:** Transfiere a la cola de agentes o a los flujos de contingencia de operadores [Operator Not Found](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-not-found.md) y [Operator Out Schedule](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/operator-out-schedule.md).
