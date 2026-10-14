# 1 - Saldos - Inicio

Entrada cuando el cliente elige **Consulta de Saldos**. Deja el trámite en contexto `saldos` y pasa al consentimiento de datos.

## Datos de entrada

- Teléfono de la sesión.
- `op_menu`: valor `saldos` elegido en [Menú principal - Bienvenida](./0-3-menu-principal-bienvenida.md).

## 1. Contexto

1. Marcador `Saldos Menu`.
2. `contexto` = `saldos`.

## Salida

- Continúa en [LOPD - Inicio](./0-1-lopd-inicio.md). Con `contexto` = `saldos`, al aceptar la política el trámite vuelve a [1 - Saldos - Menú](./1-saldos-menu.md).
- Si se abre sin el menú, vuelve a [0 - Inicio](./0-inicio.md).
