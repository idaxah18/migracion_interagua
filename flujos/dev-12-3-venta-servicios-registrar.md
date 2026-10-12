# Venta de Servicios - Registrar Solicitud (Entorno de Desarrollo)

Este flujo es el paso culminante en el entorno de desarrollo para la generación formal de órdenes de inspección y contratación de servicios especiales de ingeniería. Permite al cliente incorporar referencias de ubicación y observaciones adicionales, valida que el contrato no posea impedimentos comerciales o planillas impagas y registra la orden de trabajo (`PACKAGE_ID`) en los sistemas centrales de Interagua.

## Datos que debe traer al entrar

- `numeroContrato`: Contrato confirmado del cliente.
- `identificacion`: Cédula o RUC del titular.
- `servicio_selected`: Datos técnicos del servicio contratado.
- `departamento_selected`: Tarifa o modalidad de inspección acordada.
- `correo_selected`: Correo electrónico autenticado mediante OTP.
- `datos_cliente`: Nombre del beneficiario y dirección física del predio.

---

## Paso 1. Consulta de observaciones y comentarios

El bot consulta mediante botones si el cliente desea ingresar alguna indicación especial para la cuadrilla técnica:

> ¿Deseas agregar algún comentario o referencia adicional sobre tu solicitud?

Opciones:
1. **Sí** (Desea ingresar detalles)
2. **No** (Continuar directamente)

### Validación del comentario:
Si el usuario responde **Sí**, escribe el texto en el chat:
- **Regla de caracteres permitidos:** Solo se admiten letras, números, signos de puntuación básicos y caracteres comunes en direcciones ecuatorianas (`^[a-zA-ZáéíóúÁÉÍÓÚñÑ0-9\s.,;:()\-_/°"#%&¡!¿?@]+$`).
- **Bloqueo de enlaces externos:** Se rechaza de forma estricta el ingreso de direcciones web o URLs (`http://`, `https://`, `www.`).
- **Formateo del comentario:** Se antepone el prefijo de WhatsApp y el teléfono del usuario, limitando la longitud máxima a 150 caracteres:
  `WH 5939XXXXXXXX - {OBSERVACION}`.

---

## Servicio: Registrar Solicitud de Venta de Servicios (Desarrollo)

El sistema envía la orden de compra y solicitud de inspección a los servidores de desarrollo de Interagua:

- **Nombre de la herramienta:** Get Solicitud Venta Servicios Ingenieria
- **ID de la herramienta:** `6439`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "env": "Desarrollo",
    "contrato": "<numeroContrato>",
    "actividad": "<servicio_selected.CODIGO>",
    "comentario": "<comentario>",
    "comentario2": "<departamento_selected.CODIGO>",
    "comentario3": "<departamento_selected.VALOR>",
    "identificacion": "<identificacion>",
    "telefonoContacto": "<telefono_contacto>",
    "correoElectronico": "<correo_selected>",
    "nombreBeneficiario": "<datos_cliente.NOMBRE_BENEFICIARIO>",
    "direccionBeneficiario": "<datos_cliente.DIRECCIONTRADICIONAL>",
    "identificacionBeneficiario": "<datos_cliente.IDENTIFICACION_BENEF>"
  }
  ```

---

## Manejo de respuestas y reglas de negocio

### 1. Registro exitoso:
Cuando el sistema genera la orden de trabajo, notifica de inmediato al cliente con su número de trámite oficial:

> Tu solicitud {{$memory.response_registro.data.PACKAGE_ID}} ha sido generada con éxito. En las próximas horas nos contactaremos contigo para coordinar la inspección.

A continuación, marca el punto de control `Venta Servicios - Ex` y deriva a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).

### 2. Contrato con valores vencidos (Código de error `20201`):
Si el contrato mantiene planillas pendientes de pago, el sistema de Interagua no permite contratar servicios adicionales y despliega un menú con botones:

- **Opción de pago:** Permite ir a cancelar la deuda en [3 - Pago de Planilla - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/3-pago-de-planilla-inicio.md) (Skill `51053`).
- **Finalizar:** Cierra la gestión comercial.

### 3. Otros rechazos técnicos o comerciales:
Cualquier otro código de rechazo es transferido a [Mensaje Error Personalizado](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-personalizado.md) (Skill `51091`) para explicar la novedad al cliente.

---

## Salidas del flujo

- **Trámite finalizado con éxito:** Transfiere a [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`).
- **Derivación a pago de facturas:** Transfiere a [3 - Pago de Planilla - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/3-pago-de-planilla-inicio.md) (Skill `51053`).
- **Fallas técnicas del servidor:** Deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
