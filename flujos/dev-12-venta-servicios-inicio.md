# Venta de Servicios - Inicio (Entorno de Desarrollo)

Este flujo es la puerta de entrada en el entorno de desarrollo para la solicitud y contratación de servicios especiales de ingeniería y mantenimiento comercial de Interagua (tales como limpieza y vaciado de pozos sépticos, desinfección de cisternas y emisión de certificados de trampa de grasa).

Su función principal es configurar los parámetros técnicos de prueba, definir los textos dinámicos de consulta y derivar al cliente a la identificación de su contrato en el entorno de desarrollo.

## Datos que debe traer al entrar

- No requiere datos previos obligatorios. Se accede desde el menú principal de autoservicio o enlaces de prueba comercial.

## Configuración de memoria y servicio

Al inicializarse, el flujo define las variables de entorno de pruebas:

- **Contexto del trámite:** `contexto = "venta_servicios"`
- **URL del servicio SOAP en desarrollo:** `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`
- **Punto de control (Checkpoint):** `Venta Servicios - In`

### Mensajes dinámicos configurados para la consulta de contrato:
1. **Pregunta del menú de consulta:**
   > ¿Quieres continuar con tu número de contrato o número de cédula?
2. **Cabecera de la lista de contratos:**
   > A continuación te presentamos los contratos y direcciones asociados a tu identificación. 
   > 
   > ¿Sobre cuál quieres continuar?

---

## Salida y navegación

- **Derivación a consulta de contratos:**
  Una vez cargadas las variables y los textos en memoria, el flujo transfiere al usuario a [Dev Consulta Contratos - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/dev-consulta-contratos-inicio.md) (Skill `51897`) para individualizar la cuenta donde se prestará el servicio.
- **Manejo de errores:**
  Si ocurre una falla en la preparación de las variables, deriva a [Mensaje Error Servicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/mensaje-error-servicio.md) (Skill `51045`).
