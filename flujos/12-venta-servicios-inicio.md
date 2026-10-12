# 12 - Venta Servicios - Inicio

Punto de entrada para la contratación y solicitud de servicios comerciales de ingeniería ofrecidos por Interagua (tales como limpieza de pozos sépticos, mantenimiento de trampas de grasa o inspecciones especializadas). Configura los textos interactivos de consulta y deriva al usuario a la validación de la política de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Venta de Servicios* desde el Menú Principal o derivación por palabras clave vinculadas a servicios de ingeniería sanitaria.

## Paso 1. Marcador analítico y configuración de parámetros

Al iniciar el flujo:
1. Registra el hito analítico: `Venta Servicios - In`.
2. Establece las variables en memoria:
   - `contexto`: `venta_servicios`
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

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto `venta_servicios`.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
