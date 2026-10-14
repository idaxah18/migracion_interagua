# Convenio de pago

Permite financiar la deuda de un contrato: identifica la cuenta, confirma elegibilidad, elige cuota inicial y plazo, simula las cuotas y, si el cliente acepta, emite el convenio y entrega el cupón de pago.

## Datos de entrada

- Opción elegida: convenio de pago (menú principal, saldos o requisitos).
- Teléfono de contacto del cliente, cuando ya está registrado.

## 1. Consentimiento

Se solicita la aceptación de tratamiento de datos. Ver [LOPD - Inicio](./0-1-lopd-inicio.md).

Con consentimiento vigente se identifica el contrato. Sin consentimiento el trámite no avanza.

## 2. Identificación del contrato

El cliente elige buscar por contrato o por cédula/RUC. La captura y la validación están en [Consulta de contrato](./consulta-contratos-inicio.md), con estos textos:

> ¿Quieres realizar un convenio de pago con tu número de cédula o con tu número de contrato?

> A continuación te presentamos los contratos y direcciones asociados a tu identificación.
>
> ¿Sobre cuál quieres realizar tu convenio de pago?

Contrato válido: se guarda número de contrato, identificación y dirección, y se continúa en elegibilidad.

## 3. Elegibilidad

Se confirma el contrato y la dirección:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:
>
> *Contrato No. {numeroContrato} - {direccion}*
>
> ¿Estás de acuerdo con la información ingresada?

- **Sí:** se consulta la deuda y si el contrato puede simularse por este canal. Servicio: [getInfoSimulacionConvenio](../servicios/getInfoSimulacionConvenio.md).
- **No:** se vuelve a [identificar el contrato](#3-identificación-del-contrato).

Resultado de la consulta:

| Condición | Qué ve el cliente |
| --------- | ----------------- |
| Código `3303` | Debe ingresar la información requerida por el contrato |
| Sin registros | No se encontró información para el contrato ingresado |
| Deuda pendiente ≤ 0 | Estimado cliente hemos verificado que al momento no tienes una deuda con Interagua |
| No aplica simulación por canal virtual | Tu cuenta no cumple con las condiciones para realizar un convenio de pago, por favor comunícate con nosotros llamando al 134 |
| Otro error | El servicio no está disponible. Inténtalo más tarde |

Esos casos cierran el trámite con [Mensaje de error personalizado](./mensaje-error-personalizado.md).

Si aplica, se conservan deuda pendiente, código de producto, código de cliente y código de dirección, y se abre la [selección de plazo y cuota](./2-convenio-seleccion-plazo-cuota.md).

## 4. Simulación

Al volver de la selección de plazo y cuota:

1. Cuota inicial = deuda pendiente × porcentaje / 100, con dos decimales.
2. Se consulta la cuota mensual. Servicio: [getValorCuotaAproximada](../servicios/getValorCuotaAproximada.md).
3. Se entrega el PDF de términos y condiciones del convenio.
4. Se muestra el resumen:

> ¡Perfecto! Te presento las cuotas y los términos y condiciones del convenio simulado:
> ▪ Cuota inicial: ${cuotaInicial}
> ▪ Cuota mensual aproximada: ${cuotaAproximada}
>
> ¿Estás de acuerdo?

- **Sí:** se genera el convenio.
- **No:** no se emite el convenio y se pregunta si necesita algo más.
  - **Cambiar plazo o cuota:** se vuelve a invocar solo [Selección de plazo y cuota](./2-convenio-seleccion-plazo-cuota.md). Al terminar, se repite esta simulación con los nuevos valores. No se vuelve a confirmar el contrato ni a consultar la elegibilidad.
  - **Contactar un asesor:** [Ayuda personalizada](./ayuda-personalizada.md).
  - **No, gracias:** [Adiós](./adios.md).

Si la simulación no devuelve cuota: *No se encontró valor de cuota para el plazo y deuda indicado.* Se muestra [Mensaje de error personalizado](./mensaje-error-personalizado.md) o [Mensaje de error de servicio](./mensaje-error-servicio.md).

## 5. Generación del convenio

1. Se confirma que el contrato puede generar el convenio. Servicio: [getConfirmacionGeneracionConvenio](../servicios/getConfirmacionGeneracionConvenio.md). Si no aplica: *Tu cuenta no cumple con las condiciones para realizar un convenio de pago, por favor comunícate con nosotros llamando al 134.*
2. Se obtienen el plan de financiación y si financia deuda vencida. Servicio: [getParametroFinanciacionGeneracionConvenio](../servicios/getParametroFinanciacionGeneracionConvenio.md).
3. Se listan los conceptos de deuda del contrato. Servicio: [getListadoConceptosDeudaPorContrato](../servicios/getListadoConceptosDeudaPorContrato.md).
4. Se arma el comentario `WH {telefono} Financiacion de deuda` y la fecha del día (Ecuador). El teléfono es el de la sesión.
5. Se emite el convenio. Servicio: [generacionConvenio](../servicios/generacionConvenio.md).

| Código | Mensaje |
| ------ | ------- |
| `901705` | No hay deuda financiable |
| `903357` | Tienes un reclamo en proceso. Para continuar comunícate al 134 |
| `919317` | Su contrato no aplica para convenio de pago. Contáctese al 134 |
| Sin número de solicitud | Lo sentimos, hemos tenido un error al generar tu convenio de pago |

6. Con el número de solicitud se obtiene el cupón y la fecha límite. Servicio: [getFormatoCuponFinanciacion](../servicios/getFormatoCuponFinanciacion.md).

> ¡Listo! El número de tu cupón generado es *{numeroCupon}*.
>
> Recuerda acercarte con este número en los puntos de pagos autorizados.
>
> {texto de vigencia}

Si hay fecha límite: *Este comprobante tiene validez hasta el {fecha}, de no ser pagado en este plazo se anulará este convenio.* Si no hay fecha válida, se indica la fecha límite del convenio.

## Salida

- Convenio emitido: [Atención adicional](./atencion-adicional.md).
- No elegible o error de negocio: [Mensaje de error personalizado](./mensaje-error-personalizado.md).
- Falla del servicio: [Mensaje de error de servicio](./mensaje-error-servicio.md).
- Reinicio: [Inicio](./0-inicio.md).
