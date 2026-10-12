# 0.2 - LOPD - Deriva Contexto

Enrutador central post-autorización de datos. Una vez que el usuario cuenta con el consentimiento LOPD verificado o renovado, este flujo lee el trámite que el cliente intentaba realizar originalmente y lo redirige al flujo de destino correspondiente sin perder el estado de su consulta.

## Datos que debe traer al entrar

- Variable de contexto: `{{$memory.contexto}}` (cadena de texto que indica qué opción detonó el flujo).
- Parámetros de consentimiento: `{{$memory.paramsLODP}}` (si se generó nuevo consentimiento en el paso anterior).
- Bandera de control: `{{$memory.alreadyAskedTyC}}` = `true`.

## Paso 1. Evaluación de contexto

El flujo contiene un evaluador de condiciones múltiples que analiza el valor exacto de `{{$memory.contexto}}` en el siguiente orden:

| Valor de `contexto` | Trámite o Destino | Flujo al que deriva | Skill ID |
|---|---|---|---|
| `menu_principal` | Menú de bienvenida e inicio general | [0.3 - Menú Principal - Bienvenida](./0-3-menu-principal-bienvenida.md) | `51161` |
| `saldos` | Consulta de saldo de agua o TRB | [1 - Saldos - Menu](./1-saldos-menu.md) | `51069` |
| `convenio_pago` | Simulación y generación de convenios | [2.1 - Convenio de Pago - Contratos](./2-1-convenio-de-pago-contratos.md) | `51099` |
| `alcantarillado_sanitario` | Solicitudes y limpieza de red sanitaria | [4.1 - Alcantarillado Sanitario - Configuración](./4-1-alcantarillado-sanitario-configuracion.md) | `51101` |
| `fuga_agua` | Reporte de fuga en vía pública o medidor | [5.1 - Fugas de Agua - Configuración](./5-1-fugas-de-agua-configuracion.md) | `51102` |
| `alta_facturacion` | Reclamo por cobro elevado o consumo atípico | [6.1 - Alta Facturación - Contratos](./6-1-alta-facturacion-contratos.md) | `51090` |
| `afectacion_servicio` | Consulta de cortes o trabajos programados | [7.1 - Afectación Servicio - Contratos](./7-1-afectacion-servicio-contratos.md) | `51106` |
| `descargar_facturas` | Emisión de enlace para PDF de factura | [8.1 - Descargar Facturas - Enlace](./8-1-descargar-facturas-enlace.md) | `51193` |
| `consulta_solicitudes` | Seguimiento de tickets o trámites previos | [9.1 - Consulta Solicitudes - Solicitudes](./9-1-consulta-solicitudes-solicitudes.md) | `51089` |
| `video_asistencia` | Soporte guiado o videollamada interactiva | [_ Video bot](./video-bot.md) | `51061` |
| `consulta_requisitos` | Requisitos para trámites ciudadanos | [16.1 - Consulta Requisitos - Requisitos](./16-1-consulta-requisitos-requisitos.md) | `51164` |
| `aguas_lluvia` | Reportes de drenaje y sumideros pluviales | [10.1 - Aguas lluvias - Configuración](./10-1-aguas-lluvias-configuracion.md) | `51109` |
| `faltante_tapa` | Reporte de tapas de alcantarilla sustraídas o dañadas | [11.1 - Faltante de Tapa - Configuración](./11-1-faltante-de-tapa-configuracion.md) | `51110` |
| `venta_servicios` | Compra e instalación de servicios técnicos | [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) | `51072` |
| `certificados_administrativos` | Solicitud y emisión de certificados | [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) | `51072` |
| `reclamos_general` | Menú agrupador de quejas e incidentes | [Reclamos - Menu](./reclamos-menu.md) | `51185` |

## Salida

- Si el valor de `contexto` coincide con cualquiera de los anteriores, el usuario ingresa de inmediato al flujo correspondiente.
- Si por alguna razón la variable `contexto` está vacía o no coincide con ninguna regla, el sistema utiliza el camino por defecto:
  - Transfiere a: [0 - Inicio](./0-inicio.md) (Skill ID `51065`) para reiniciar la interacción de forma segura.
