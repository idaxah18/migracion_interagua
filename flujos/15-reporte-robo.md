# 15 - Reporte Robo

Canal prioritario de atención para reportes urgentes por sustracción, robo o daño flagrante de medidores de agua potable e infraestructura pública. Transfiere la conversación en caliente a un operador humano del centro de contacto de Interagua mediante la pasarela de enrutamiento Jelou Connect.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Reporte de Robo* o palabras clave de alta criticidad (robo de medidor, medidor sustraído).

## Paso 1. Notificación de transferencia humana

El bot notifica al cliente que será derivado con un asesor en tiempo real:

> Te estamos transfiriendo con un asesor. Por favor, mantente en línea para que podamos atender tu requerimiento.

## Paso 2. Enrutamiento en vivo mediante Jelou Connect

El sistema ejecuta la conexión con la cola de agentes humanos:

Configuración de conexión:
- **Prioridad:** `0` (Máxima prioridad de atención en cola).
- **Estrategia de asignación:** `shuffle` (Distribución balanceada directa entre asesores disponibles).
- **Atención en falla:** Genera conversación no atendida si no existen operadores libres.

## Salida y contingencias

1. **Conexión exitosa (`JelouConnect`):** El cliente pasa a ser atendido en vivo por un operador humano en la plataforma Jelou.
2. **Sin operadores disponibles (`OPERATOR_NOT_FOUND` / `UNKNOWN_ERROR`):**  
   Transfiere a [_ Operator Not Found](./operator-not-found.md) (Skill ID `51040`) para registrar el requerimiento pendiente.
3. **Fuera del horario de atención del contact center (`OPERATORS_NOT_IN_SCHEDULER`):**  
   Transfiere a [_ Operator Out Schedule](./operator-out-schedule.md) (Skill ID `51041`) para informar los horarios de guardia y canales telefónicos 24/7.
4. **En caso de cancelación o reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
