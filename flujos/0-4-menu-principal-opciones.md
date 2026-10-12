# 0.4 - Menú Principal - Opciones

Presenta el menú secundario ("Otras Opciones") para trámites complementarios y subcategorías de servicio (Agua y Saneamiento, Solicitudes, Requisitos y Videobot). Además, se encarga de guardar formalmente en la base de datos la aceptación de la política LOPD si estaba pendiente de registro.

## Datos que debe traer al entrar

- Parámetros de consentimiento: `{{$memory.paramsLODP}}` (si se generaron en el flujo de LOPD).
- Bandera de inserción: `{{$memory.requiredQuestion}}` (indica si se requiere registrar la aceptación en base de datos).
- Contexto de navegación: WhatsApp / Web.

## Paso 1. Menú de Otras Opciones

El bot presenta la lista complementaria con el texto:

> Selecciona una de las siguientes opciones:

Opciones desplegadas en pantalla:

1. **Consulta de Solicitudes** (`consulta_solicitudes`): Revisa el estado de tus solicitudes de servicio.
2. **Requisitos de Trámites** (`consulta_requisitos`): Conoce los requisitos para realizar trámites.
3. **Videobot** (`video_asistencia`): Asistencia virtual para resolver tus dudas sobre el servicio.
4. **Venta de servicios** (`venta_servicios`): Solicita servicios técnicos y asesorías especializadas.
5. **Certificados Administr.** (`certificados_administrativos`): Solicita Certificado de No Adeudar o Activación de Servicios.
6. **Agua y Saneamiento**: Despliega el submenú de incidencias técnicas:
   - Agua Potable (`fuga_agua`)
   - Aguas Lluvias (`aguas_lluvia`)
   - Alcantarillado Sanitario (`alcantarillado_sanitario`)
7. **Volver al menú**: Regresa al [0.3 - Menú Principal](./0-3-menu-principal.md).

## Paso 2. Registro de consentimiento LOPD (si aplica)

Al seleccionar cualquier opción del menú, si `{{$memory.requiredQuestion}}` es `true`, el sistema ejecuta en segundo plano el registro de consentimiento en la base de datos:

Servicio: **SavePoliticaDatos** (Tool ID `1545`)

Headers:

apikey = Bearer Token Jelou  
Content-type = application/json  

Endpoint:

`POST https://api.jelou.ai/v2/databases/1611/rows`

Request:

```json
{
  "canal": "{{$memory.canal}}",
  "fecha": "<fecha>",
  "hora": "<hora>",
  "userId": "<telefono>",
  "userName": "<nombre>",
  "identificacion": "<identificacion>"
}
```

Respuesta esperada:

- `saveLOPD`: Confirmación del registro insertado con ID en la tabla 1611.
- Una vez guardado, el flujo continúa de inmediato sin demoras para el usuario.

## Salida

El flujo evalúa la opción elegida (`op_menu`) y transfiere la conversación a:

- **Consulta de Solicitudes:** Deriva a [_ 9.0 - Consulta Solicitudes - Inicio](./9-0-consulta-solicitudes-inicio.md) (Skill ID `51059`).
- **Requisitos de Trámites:** Deriva a [_ 16 - Consulta Requisitos - Inicio](./16-consulta-requisitos-inicio.md) (Skill ID `51060`).
- **Videobot:** Deriva a [_ Video Bot - Inicio](./video-bot-inicio.md) (Skill ID `51122`).
- **Venta de Servicios:** Deriva a [_ 12 - Venta Servicios - Inicio](./12-venta-servicios-inicio.md) (Skill ID `51077`).
- **Certificados Administrativos:** Deriva a [_ 13 - Certificados - Inicio](./13-certificados-inicio.md) (Skill ID `51084`).
- **Agua Potable:** Deriva a [_ 5.0 - Fugas de Agua - Inicio](./5-0-fugas-de-agua-inicio.md) (Skill ID `51055`).
- **Aguas Lluvias:** Deriva a [_ 10.0 - Aguas lluvias - Inicio](./10-0-aguas-lluvias-inicio.md) (Skill ID `51062`).
- **Alcantarillado Sanitario:** Deriva a [_ 4.0 - Alcantarillado Sanitario - Inicio](./4-0-alcantarillado-sanitario-inicio.md) (Skill ID `51054`).
- **Volver al Menú:** Deriva a [0.3 - Menú Principal](./0-3-menu-principal.md) (Skill ID `51068`).
