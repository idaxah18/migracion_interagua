# Atención Adicional

Punto de cierre intermedio que consulta al cliente si requiere gestionar algún otro requerimiento tras haber completado un trámite, ofreciendo retornar al Menú Principal o finalizar la conversación cordialmente.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Trámite previo completado exitosamente.

## Paso 1. Consulta de requerimiento adicional

El bot envía un mensaje interactivo con botones:

> Fue un placer ayudarte, si necesitas algo adicional por favor indícanos

Opciones con botones:
1. **Menú Principal:** Transfiere al catálogo de opciones [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md).
2. **No, gracias:** Transfiere al cierre de sesión [_ Adiós](./adios.md).

## Salida

- **Continuar navegando:** [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md).
- **Finalizar atención:** [_ Adiós](./adios.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
