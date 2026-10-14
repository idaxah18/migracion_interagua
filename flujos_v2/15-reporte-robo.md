# 15 - Reporte de robo

Atiende el reporte urgente de robo o daño de un medidor o de infraestructura. Pasa la conversación a un asesor con prioridad alta y reparto entre los operadores disponibles.

## Datos de entrada

- Teléfono de la sesión.
- Llegada por la opción *Reporte de Robo* o por palabras como robo de medidor.

## 1. Aviso

> Te estamos transfiriendo con un asesor. Por favor, mantente en línea para que podamos atender tu requerimiento.

## 2. Transferencia

Se intenta la conexión con la cola de asesores. Si no hay operadores libres, el caso queda registrado como no atendido.

## Salida

- Asesor asignado: la conversación sigue con esa persona.
- Sin operadores: [Operador no disponible](./operator-not-found.md).
- Fuera de horario: [Asesores fuera de horario](./operator-out-schedule.md).
- Cancelación o reinicio: [0 - Inicio](./0-inicio.md).
