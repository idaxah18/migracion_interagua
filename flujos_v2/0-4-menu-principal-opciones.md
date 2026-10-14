# 0.4 - Menú Principal - Opciones

Presenta el menú secundario ("Otras Opciones") para trámites complementarios y subcategorías de servicio (Agua y Saneamiento, Solicitudes, Requisitos y Videobot).

## Datos que debe traer al entrar

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
7. **Volver al menú**: Regresa al [0.3 - Menú Principal](../flujos/0-3-menu-principal.md).

## Salida

El flujo evalúa la opción elegida (`op_menu`) y transfiere la conversación a:

- **Consulta de Solicitudes:** Deriva a [_ 9.0 - Consulta Solicitudes - Inicio](./9-0-consulta-solicitudes-inicio.md).
- **Requisitos de Trámites:** Deriva a [_ 16 - Consulta Requisitos - Inicio](../flujos/16-consulta-requisitos-inicio.md).
- **Videobot:** Deriva a [_ Video Bot - Inicio](./video-bot-inicio.md).
- **Venta de Servicios:** Deriva a [_ 12 - Venta Servicios - Inicio](./12-venta-servicios-inicio.md).
- **Certificados Administrativos:** Deriva a [_ 13 - Certificados - Inicio](./13-certificados-inicio.md).
- **Agua Potable:** Deriva a [_ 5.0 - Fugas de Agua - Inicio](./5-0-fugas-de-agua-inicio.md).
- **Aguas Lluvias:** Deriva a [_ 10.0 - Aguas lluvias - Inicio](./10-0-aguas-lluvias-inicio.md).
- **Alcantarillado Sanitario:** Deriva a [_ 4.0 - Alcantarillado Sanitario - Inicio](./4-0-alcantarillado-sanitario-inicio.md).
- **Volver al Menú:** Deriva a [0.3 - Menú Principal](../flujos/0-3-menu-principal.md).
