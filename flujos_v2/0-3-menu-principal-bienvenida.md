# Menú Principal - Bienvenida

Presenta el saludo inicial personalizado de la asistente virtual "Iris" y despliega la lista interactiva con las principales opciones y trámites disponibles para los clientes de Interagua.

## Datos que debe traer al entrar

- Nombre del cliente: `{{nombreCliente}}` (si está registrado o identificado).
- Canal de comunicación: Facebook / Instagram / Web.

## Paso 1. Selección aleatoria del saludo

Para ofrecer una experiencia conversacional natural y variada, el sistema selecciona al azar una de las siguientes tres frases de bienvenida y la guarda en la variable `msm_menu_principal`:

1. > Hola {{$memory.nombreCliente}}, es un placer saludarte. Soy Iris y estoy lista para ayudarte.😊
2. > Hola {{$memory.nombreCliente}}, es un gusto tenerte por aquí!. Soy Iris. En qué puedo ayudarte? 😊
3. > ¡Hola {{$memory.nombreCliente}} que tal! Soy Iris y estoy lista para servirte.😀

*(Si el nombre del cliente no está disponible, se omite de forma limpia).*

## Paso 2. Presentación del Menú Interactivo

El asistente envía un mensaje de lista interactiva en WhatsApp con el saludo seleccionado como encabezado y 10 opciones de servicio organizadas:

| # | Opción en Pantalla | Descripción mostrada al usuario | Valor interno (`op_menu`) |
|---|--------------------|---------------------------------|-----------------------------|
| 1 | **Consulta de Saldos** | Verifica el saldo de tu cuenta de agua fácilmente | `saldos` |
| 2 | **Pagos en Línea** | Realiza tus pagos de agua de forma segura y rápida | `pago_planilla` |
| 3 | **Convenio de Pago** | Realiza un acuerdo de pago de tus facturas de agua potable | `convenio_pago` |
| 4 | **Afectación del Servicio** | Consulta afectaciones en el servicio de agua potable | `afectacion_servicio` |
| 5 | **Agua Potable** | Reporta fugas o baja presión de agua | `fuga_agua` |
| 6 | **Aguas Lluvias** | Reportar limpiezas al sistema de alcantarillado pluvial | `aguas_lluvia` |
| 7 | **Alcantarillado Sanitario** | Reportar limpiezas al sistema de alcantarillado sanitario | `alcantarillado_sanitario` |
| 8 | **Reclamos por Facturación** | Factura alta; cuide sus instalaciones para evitar consumos innecesarios | `alta_facturacion` |
| 9 | **Descargar Facturas** | Descarga tus facturas de agua de manera sencilla | `descargar_facturas` |
| 10 | **Otras Opciones** | Ver opciones adicionales del menú | `otras_opciones` |

## Salida

Cuando el usuario selecciona una de las opciones de la lista, el flujo asigna el valor seleccionado a la variable `op_menu` y transfiere de inmediato la atención al flujo correspondiente:

- **Consulta de Saldos:** Deriva a [_ 1 - Saldos - Inicio](./1-saldos-inicio.md).
- **Pagos en Línea:** Deriva a [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md).
- **Convenio de Pago:** Deriva a [Convenio de pago](./2-convenio-de-pago.md).
- **Afectación del Servicio:** Deriva a [_ 7.0 - Afectación Servicio - Inicio](./7-0-afectacion-servicio-inicio.md).
- **Agua Potable (Fugas):** Deriva a [_ 5.0 - Fugas de Agua - Inicio](./5-0-fugas-de-agua-inicio.md).
- **Aguas Lluvias:** Deriva a [_ 10.0 - Aguas lluvias - Inicio](./10-0-aguas-lluvias-inicio.md).
- **Alcantarillado Sanitario:** Deriva a [_ 4.0 - Alcantarillado Sanitario - Inicio](./4-0-alcantarillado-sanitario-inicio.md).
- **Reclamos por Facturación:** Deriva a [_ 6.0 - Alta Facturación - Inicio](./6-0-alta-facturacion-inicio.md).
- **Descargar Facturas:** Deriva a [_ 8.0 - Descargar Facturas - Inicio](./8-0-descargar-facturas-inicio.md).
- **Otras Opciones:** Deriva a [_ 0.4 - Menú Principal - Opciones](./0-4-menu-principal-opciones.md).
