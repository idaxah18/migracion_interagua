# Venta de Servicios - Departamentos y Cotización (Entorno de Desarrollo)

Este flujo consulta las modalidades específicas, clasificaciones comerciales y costos estimados asociados al servicio seleccionado por el usuario en el entorno de desarrollo. Si existen múltiples actividades o capacidades operativas (por ejemplo, volumen del pozo o giro del negocio), despliega un menú de selección; si existe una tarifa única, la asigna automáticamente y presenta el costo estimado para su aceptación previa.

## Datos que debe traer al entrar

- `identificacion`: Cédula o RUC del cliente.
- `servicio_selected`: Objeto en memoria del servicio elegido en el paso previo.
- `servicio_selected.ID_TIPO_INSPECCION`: Código de inspección técnica.
- `is_trampa_grasa`: Booleano que define si corresponde a trampa de grasa.

---

## Servicio: Consulta de departamentos y tarifas (Desarrollo)

Recupera las variantes tarifarias y actividades económicas aplicables al servicio en el predio.

- **Nombre de la herramienta:** Get Consulta Departamentos
- **ID de la herramienta:** `6418`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **Parámetros de entrada:**
  ```json
  {
    "id_tipo_inspeccion": "<servicio_selected.ID_TIPO_INSPECCION>",
    "identificacion": "<identificacion>",
    "env": "Desarrollo"
  }
  ```
- **Respuesta esperada:** Lista de opciones con `CODIGO`, `DESCRIPCION` y valor de la tasa o inspección (`VALOR`).

---

## Paso 1. Selección de modalidad o actividad

El sistema evalúa la cantidad de registros devueltos:

- **Si solo existe una opción:**
  Se selecciona de forma automática sin intermediación del cliente y se avanza directamente a la presentación de la tarifa.
- **Si existen múltiples opciones:**
  Se construye un menú interactivo:
  - Para trampas de grasa:
    > Seleccione la actividad de su establecimiento: 
    > 
    > 1 - {ACTIVIDAD_1}  
    > 2 - {ACTIVIDAD_2}  
    > ...  
    > *Ingresa 0 para salir del menú*
  - Para servicios generales:
    > Seleccione una de las opciones disponibles: 
    > 
    > 1 - {OPCION_1}  
    > 2 - {OPCION_2}  
    > ...  
    > *Ingresa 0 para salir del menú*

### Reglas de validación:
- Si el usuario digita `0`, abandona el menú y deriva a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).
- Si digita un número incorrecto, el bot responde:
  > La opción que ingresaste no es correcta. Por favor. inténtalo nuevamente.

---

## Paso 2. Presentación y aprobación del costo estimado

Una vez definida la opción, el bot informa el valor referencial y solicita la aceptación formal del cliente mediante botones:

> Este servicio tiene un costo estimado de $ {departamento.VALOR}.
> {DESCRIPCION_DETALLADA}
> 
> ¿Estás de acuerdo?

Opciones en botones:
1. **Sí** (Acepta el costo estimado)
2. **No** (Rechaza el valor del servicio)

### Si el cliente responde que No:
El bot finaliza respetuosamente la gestión comercial:
> Entiendo que no te encuentres de acuerdo. Si cambias de opinión, no dudes en escribirme. 👋
Y transfiere a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).

---

## Salidas del flujo

- **Aceptación para servicios generales:** Transfiere al flujo de autenticación OTP [OTP - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/otp-inicio.md) (Skill `51079`) o directamente a registro [Dev 12.3 - Venta Servicios - Registrar](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/dev-12-3-venta-servicios-registrar.md) (Skill `51902`).
- **Aceptación para trampas de grasa:** Transfiere a [12.5 - Venta Servicios - Trampa Grasa Registrar](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/12-5-venta-servicios-trampa-grasa-registrar.md) (Skill `51083`).
- **Sin departamentos o error técnico:** Deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
