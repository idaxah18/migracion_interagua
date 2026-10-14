# 1.1 - Saldos Consumo Agua - Inicio

Prepara los textos de la consulta de contratos para el saldo de agua y pasa a identificar la cuenta.

## Datos de entrada

- Teléfono de la sesión.
- Consentimiento LOPD vigente.
- `contexto`: `saldos`, asignado en [1 - Saldos - Inicio](./1-saldos-inicio.md).

## 1. Preparación

1. Marcador `Saldos Agua`.
2. Imagen: `https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10488%2Fimage%2Fd776e4ae-24d8-46d1-9ec7-cfaa988da276-1752162542075.jpeg`
3. Pausa de 3 segundos.
4. Textos que usa [Consulta de contratos](./consulta-contratos-inicio.md) en este trámite. Se asignan aquí:

`msm_btn_consultaContrato`:

> ¿Quieres consultar tu saldo de consumo de agua con tu número de contrato o número de cédula?

`msm_btn_listaContratos`:

> A continuación te presentamos los contratos y direcciones asociados a tu identificación.
>
> ¿Sobre cuál quieres consultar tu saldo de consumo de agua?

## Salida

- Continúa en [Consulta de contratos - Inicio](./consulta-contratos-inicio.md). Con `contexto` = `saldos`, [Consulta de contratos - Derivación](./consulta-contratos-derivacion.md) entrega el contrato a [1.1 - Saldos Consumo Agua - Confirmación](./1-1-saldos-consumo-agua-confirmacion.md).
- Si se abre sin el menú: [0 - Inicio](./0-inicio.md).
