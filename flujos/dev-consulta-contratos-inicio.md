# Consulta de contratos - Inicio (Entorno de Desarrollo)

Este flujo es la versión de desarrollo y homologación técnica del componente de [Consulta de contrato](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/consulta-contratos-inicio.md). Permite identificar el contrato o la identificación del cliente contra los servicios del entorno de pruebas (`Desarrollo`) e incluye datos simulados (mocks) para certificar nuevos desarrollos sin impactar los sistemas comerciales en producción.

Al igual que en producción, el flujo no fija el texto del menú inicial ni el de la lista de contratos; ambos llegan parametrizados de forma dinámica según el trámite de origen.

## Datos que debe traer al entrar

- Texto del menú inicial (`msm_menu_contrato`).
- Texto de la lista de contratos (`msm_btn_listaContratos`).
- Dirección del servicio: [http://192.168.10.157/api-services-int/api/services](http://192.168.10.157/api-services-int/api/services)
- Ambiente: `Desarrollo`.
- Contexto: opción por la que llega (`saldos`, `convenio_pago`, `alta_facturacion`, `reclamo_tecnico`, `afectacion_servicio`, `venta_servicios`, etc.).

## Mensajes dinámicos según el trámite

Los mensajes del menú inicial y la cabecera de la lista se adaptan automáticamente a la gestión solicitada:

- **Saldo de consumo de agua:**
  - Menú: *¿Quieres consultar tu saldo de consumo de agua con tu número de contrato o número de cédula?*
  - Lista: *A continuación te presentamos los contratos y direcciones asociados a tu identificación. ¿Sobre cuál quieres consultar tu saldo de consumo de agua?*

- **Convenio de pago:**
  - Menú: *¿Quieres realizar un convenio de pago con tu número de cédula o con tu número de contrato?*
  - Lista: *A continuación te presentamos los contratos y direcciones asociados a tu identificación. ¿Sobre cuál quieres realizar tu convenio de pago?*

- **Alta facturación y Reclamos técnicos:**
  - Menú: *¿Quieres registrar tu reclamo con tu número de cédula o con número de contrato?*
  - Lista: *A continuación te presentamos los contratos y direcciones asociados a tu identificación. ¿Sobre cuál quieres registrar tu reclamo?*

- **Venta de servicios y certificados:**
  - Menú: *¿Quieres continuar con tu número de contrato o número de cédula?*
  - Lista: *A continuación te presentamos los contratos y direcciones asociados a tu identificación. ¿Sobre cuál quieres continuar?*

---

## Paso 1. Elegir cómo consultar

El bot despliega el menú inicial con dos opciones en botones:

1. **Contrato**
2. **Cédula o RUC**

---

## Paso 2A. Consulta directa por número de contrato

Si el usuario elige consultar por **Contrato**:
1. El bot solicita el número de contrato y lo almacena en `numeroContrato`.
2. Invoca el servicio técnico en ambiente de pruebas:

### Servicio: Get Info Contrato (Desarrollo)
- **Nombre de la herramienta:** Get Info Contrato
- **ID de la herramienta:** `2937`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **URL del servicio:** `http://192.168.10.157/api-services-int/api/services`
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "url": "http://192.168.10.157/api-services-int/api/services",
    "identificacion": "<numeroContrato>",
    "numeroContrato": "<numeroContrato>",
    "env": "Desarrollo"
  }
  ```

### Datos de prueba integrados (Mock Test Fixture):
En este entorno se dispone de un contrato de prueba preconfigurado:
- Titular: `NELSON FELIPE QUIMI CALDERON`
- Cédula: `0909914723`
- Contrato: `584924`
- Dirección: `COOPERATIVA POLO SUR / 2º CJON. 48 SO MZ. 0102 - 10`
- Categoría: `RESIDENCIAL`

---

## Paso 2B. Consulta por cédula o RUC

Si el usuario elige consultar por **Cédula o RUC**:
1. El bot solicita la identificación y la almacena en `identificacion`.
2. Invoca el servicio técnico de búsqueda de contratos asociados:

### Servicio: Get Kiosko (Desarrollo)
- **Nombre de la herramienta:** Get Kiosko
- **ID de la herramienta:** `2936`
- **Ambiente:** `Desarrollo`
- **Método HTTP:** `POST`
- **URL del servicio:** `http://192.168.10.157/api-services-int/api/services`
- **Cuerpo de la solicitud (Request):**
  ```json
  {
    "url": "http://192.168.10.157/api-services-int/api/services",
    "identificacion": "<identificacion>",
    "env": "Desarrollo"
  }
  ```

### Diccionario de errores y validaciones:
```javascript
const CODES_ERROR = {
  '913194': 'No existen contratos asociados para el número de identificación.',
  '801543': 'El contrato ingresado es incorrecto, o no tiene contratos activos.'
};
const DEFAULT_ERROR = '¡Lo sentimos!, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.';
```

---

## Paso 3. Paginación y selección de contratos

Si la búsqueda por cédula retorna múltiples contratos:
- Se agrupan en páginas de hasta **3 contratos por mensaje** para cumplir con el límite de WhatsApp.
- Cada botón muestra el número de contrato y su dirección resumida (recortada a 72 caracteres).
- Si hay más de 3 contratos, se agrega el botón interactivo **"Ver más"** para paginar.
- Al seleccionar el contrato deseado, se cargan sus datos completos en la memoria de la sesión.

---

## Salida

Una vez seleccionado y confirmado el contrato:
- El flujo transfiere al usuario a [Dev Consulta Contratos - Derivación](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/dev-consulta-contratos-derivacion.md) (o al flujo de destino correspondiente) para continuar la prueba del trámite en desarrollo.
- Si la consulta arroja error, se muestra el mensaje específico o se deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
