# 13 - Certificados - Inicio

Punto de entrada para la emisión y despacho digital de certificados oficiales emitidos por Interagua (tales como Certificado de No Adeudar o Certificado de Suscripción del Servicio). Configura los textos interactivos de consulta y deriva al usuario hacia la verificación de la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Certificados* desde el Menú Principal o derivación por palabras clave vinculadas a certificados administrativos.

## Paso 1. Marcador analítico y configuración de parámetros

Al iniciar el flujo:
1. Registra el hito analítico: `Certificados - In`.
2. Establece las variables en memoria:
   - `contexto`: `certificados_administrativos`
   - `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`

## Paso 2. Configuración de textos dinámicos de consulta

El bot inyecta en memoria las preguntas interactivas para la consulta del contrato o identificación:

- `msm_btn_consultaContrato`:
  > ¿Quieres continuar con tu número de contrato o número de cédula?
- `msm_btn_listaContratos`:
  > A continuación te presentamos los contratos y direcciones asociados a tu identificación.  
  >   
  > ¿Sobre cuál quieres continuar?

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) para validar el consentimiento de datos personales bajo el contexto `certificados_administrativos`.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
