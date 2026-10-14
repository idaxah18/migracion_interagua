# Ayuda Personalizada

Ofrece asistencia humana personalizada ante situaciones no resueltas por las respuestas automáticas, permitiendo que el usuario decida si desea ser transferido a un agente o regresar al menú principal.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Solicitud de escalamiento o excepción no controlada en flujos de autoservicio.

## Paso 1. Consulta de transferencia a operador

El bot envía la pregunta con botones:

> ¿Deseas que un asesor se comunique contigo para revisar esta novedad?

Opciones con botones:
1. **Sí:** Procede a la transferencia humana en vivo:
   > Se deriva a un asesor
   *(Conecta mediante Jelou Connect con un agente humano del contact center).*
2. **No:** Desiste de la ayuda humana:
   > De acuerdo, ha sido un placer. Recuerda que estamos para servirte.  
   *(Transfiere a [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md)).*

## Salida

- **Transferencia aprobada:** Derivación directa al operador en la plataforma de atención humana.
- **Rechazo de transferencia:** Transfiere a [_ 0.3 - Menú Principal](../flujos/0-3-menu-principal.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
