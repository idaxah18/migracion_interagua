# 0.3 - Menú Principal

Despliega el menú de navegación general cuando el usuario regresa de una consulta previa, solicita volver al inicio o escribe "Menú" durante la conversación.

## Datos que debe traer al entrar

- Nombre del cliente: `{{$memory.nombreCliente}}` (si está registrado).
- Canal: WhatsApp / Web.

## Paso 1. Mensaje de presentación del menú

El bot presenta la lista de servicios con el texto:

> ¿En qué más puedo ayudarte hoy? Selecciona una de las siguientes opciones:

## Paso 2. Opciones del Menú Principal

Muestra la lista con las opciones estándar de servicio:

1. **Consulta de Saldos** (`saldos`): Verificación de saldo de consumo o deuda pendiente.
2. **Pagos en Línea** (`pago_planilla`): Canales y pasarela de recaudación.
3. **Convenio de Pago** (`convenio_pago`): Financiamiento y facilidades de pago.
4. **Afectación del Servicio** (`afectacion_servicio`): Interrupciones y cortes programados.
5. **Agua Potable** (`fuga_agua`): Reporte de fugas o fallas de suministro.
6. **Aguas Lluvias** (`aguas_lluvia`): Mantenimiento de alcantarillado pluvial.
7. **Alcantarillado Sanitario** (`alcantarillado_sanitario`): Redes de aguas servidas.
8. **Reclamos por Facturación** (`alta_facturacion`): Valores inusuales en la planilla.
9. **Descargar Facturas** (`descargar_facturas`): Obtención de facturas electrónicas.
10. **Otras Opciones** (`otras_opciones`): Menú secundario de trámites y solicitudes.

## Salida

Al pulsar cualquiera de las opciones, transfiere inmediatamente al flujo correspondiente:

- `saldos`: [_ 1 - Saldos - Inicio](./1-saldos-inicio.md) (Skill ID `51051`).
- `pago_planilla`: [_ 3 - Pago de Planilla - Inicio](./3-pago-de-planilla-inicio.md) (Skill ID `51053`).
- `convenio_pago`: [_ 2.0 - Convenio de Pago - Inicio](./2-0-convenio-de-pago-inicio.md) (Skill ID `51052`).
- `afectacion_servicio`: [_ 7.0 - Afectación Servicio - Inicio](./7-0-afectacion-servicio-inicio.md) (Skill ID `51057`).
- `fuga_agua`: [_ 5.0 - Fugas de Agua - Inicio](./5-0-fugas-de-agua-inicio.md) (Skill ID `51055`).
- `aguas_lluvia`: [_ 10.0 - Aguas lluvias - Inicio](./10-0-aguas-lluvias-inicio.md) (Skill ID `51062`).
- `alcantarillado_sanitario`: [_ 4.0 - Alcantarillado Sanitario - Inicio](./4-0-alcantarillado-sanitario-inicio.md) (Skill ID `51054`).
- `alta_facturacion`: [_ 6.0 - Alta Facturación - Inicio](./6-0-alta-facturacion-inicio.md) (Skill ID `51056`).
- `descargar_facturas`: [_ 8.0 - Descargar Facturas - Inicio](./8-0-descargar-facturas-inicio.md) (Skill ID `51058`).
- `otras_opciones`: [_ 0.4 - Menú Principal - Opciones](./0-4-menu-principal-opciones.md) (Skill ID `51039`).
