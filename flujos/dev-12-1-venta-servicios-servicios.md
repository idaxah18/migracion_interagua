# Venta de Servicios - Catálogo de Servicios (Entorno de Desarrollo)

Este flujo despliega el catálogo de servicios técnicos, inspecciones y mantenimientos comerciales disponibles para el predio del cliente en el entorno de desarrollo. Consulta en tiempo real las características del contrato para determinar qué prestaciones son técnicamente viables y orienta al usuario según el tipo de servicio seleccionado (trampas de grasa, pozos sépticos o servicios generales).

## Datos que debe traer al entrar

- `numeroContrato`: Número de contrato confirmado del cliente.
- `identificacion`: Cédula o RUC del titular.
- `contexto`: `venta_servicios`.

---

## Servicio 1: Obtener datos comerciales del cliente (Desarrollo)

Recupera los identificadores internos del cliente y del contrato necesarios para consultar los servicios aplicables.

- **Nombre de la herramienta:** Get Datos Cliente
- **ID de la herramienta:** `6440`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **Parámetros de entrada:**
  ```json
  {
    "contrato": "<numeroContrato>",
    "identificacion": "<identificacion>",
    "env": "Desarrollo"
  }
  ```
- **Respuesta esperada:** Datos maestros del cliente, incluyendo `ID_CLIENTE`, `ID_CONTRATO`, `NOMBRE_BENEFICIARIO` y dirección del predio.

---

## Servicio 2: Obtener catálogo de tipos de servicio (Desarrollo)

Consulta el catálogo de servicios habilitados comercialmente para el predio.

- **Nombre de la herramienta:** Get Listado Tipo Servicio
- **ID de la herramienta:** `6438`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **Parámetros de entrada:**
  ```json
  {
    "idCliente": "<datos_cliente.ID_CLIENTE>",
    "idContrato": "<datos_cliente.ID_CONTRATO>",
    "env": "Desarrollo"
  }
  ```
- **Respuesta esperada:** Arreglo con la lista de servicios disponibles (`CODIGO`, `DESCRIPCION`, `EXPLICACION_BREVE`, `ID_TIPO_INSPECCION`).

---

## Paso 1. Presentación del menú interactivo de servicios

El sistema formatea la lista de servicios recuperados y la entrega al usuario mediante texto estructurado:

> Estos son nuestros servicios:
> 
> 1 - {DESCRIPCION_SERVICIO_1}  
> 2 - {DESCRIPCION_SERVICIO_2}  
> ...  
> 
> *Ingresa 0 para salir del menú*

### Reglas de selección:
- **Digitar `0`:** El usuario sale voluntariamente del menú y es transferido a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).
- **Número fuera de rango o texto:** El bot solicita ingresar una opción válida comprendida entre `1` y el número total de servicios.

---

## Paso 2. Clasificación y validación técnica del servicio

Al ingresar una opción válida, el flujo identifica el tipo de inspección:

### 1. Certificado de Trampa de Grasa (`PE_TIPO_VTA_INGE_00`):
- Fija `is_trampa_grasa = true`.
- Muestra el texto explicativo de la normativa y requisitos del servicio:
  > `{{$memory.servicio_selected.EXPLICACION_BREVE}}`
- Transfiere al usuario al flujo de verificación de identidad OTP [OTP - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/otp-inicio.md) (Skill `51080`).

### 2. Limpieza de Pozo Séptico:
- Fija `is_pozo_septico = true`.
- Despliega las condiciones técnicas obligatorias que debe cumplir el predio antes de programar el hidrocarburo o vehículo succionador:
  > Antes de continuar, verifica que tu pozo séptico cumpla con las siguientes condiciones:
  > • La abertura de acceso al pozo debe ser de al menos 40 cm x 40 cm.
  > • La tapa de acceso debe encontrarse abierta y sin obstrucciones ni daños estructurales.
  > • Debe existir una distancia máxima de 30 metros entre el pozo y el punto hasta donde pueda llegar el vehículo.
  > • El acceso debe ser adecuado para el ingreso del vehículo, considerando pendientes, callejones, condiciones del acceso vehicular y del terreno.
  > Importante: El servicio contempla únicamente la extracción de líquidos. No incluye la extracción de grasas ni aceites.
- Consulta mediante botones si el predio cumple las condiciones para continuar.

### 3. Otros servicios de ingeniería generales:
- Deriva de manera inmediata al flujo de selección de departamentos o zonas del predio.

---

## Salidas del flujo

- **Servicios generales:** Transfiere a [Dev 12.2 - Venta Servicios - Departamentos](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/dev-12-2-venta-servicios-departamentos.md) (Skill `51901`).
- **Trampas de grasa:** Transfiere a [OTP - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/otp-inicio.md) (Skill `51080`).
- **Salida / Cancelación:** Deriva a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).
- **Falla de conexión:** Deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
