# 7.1 - Afectación Servicio - Contratos

Prepara el entorno técnico para consultar interrupciones en el suministro de agua asociadas a un contrato o sector específico, configurando la URL del Web Service de Interagua y los textos interactivos antes de derivar al flujo maestro de contratos.

## Datos que debe traer al entrar

- `contexto`: `afectacion_servicio`.
- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento de datos personales (LOPD) verificado positivamente.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- **En mantenimiento (`enMantenimiento`):** Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- **Operativo (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Configuración técnica y textos personalizados

El flujo establece en la memoria técnica la URL del servicio SOAP y los textos dinámicos de consulta:

1. **Configuración de servicio:**
   - `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`

2. **Textos inyectados para el selector de contratos:**
   - `msm_btn_consultaContrato`:
     > ¿Deseas consultar si existe una afectación del servicio con tu número de cédula o con número de contrato?
   - `msm_btn_listaContratos`:
     > A continuación te presentamos los contratos y direcciones asociados a tu identificación.  
     >   
     > ¿Sobre cuál quieres consultar si existe una afectación del servicio?

## Salida

- **Continuación normal:** Transfiere a [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) (Skill ID `51072`) para solicitar el contrato o cédula y recuperar los inmuebles vinculados.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
