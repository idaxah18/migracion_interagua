# Citas y agendamiento previo (Citas)

Este flujo proporciona al cliente el enlace directo y oficial para agendar, reprogramar o consultar citas previas de atención personalizada en las agencias de Interagua o para videollamadas con un asesor de servicio.

## Datos que debe traer al entrar

- No requiere parámetros previos obligatorios.
- Se invoca cuando el usuario solicita expresamente una cita o selecciona la opción desde menús de asistencia.

## Información y enlace de agendamiento

El bot entrega el siguiente mensaje explicativo con la dirección web del portal institucional:

> Si deseas solicitar o ajustar una cita accede al siguiente enlace:
> 
> https://citasprevias.interagua.com.ec/

### Opciones que el ciudadano puede realizar en el portal:
- Reservar turno presencial en las agencias de atención al cliente (Edificio Principal, Centro, Posorja, etc.).
- Modificar o cancelar una cita agendada previamente.
- Solicitar una cita virtual por videollamada para trámites comerciales.

## Salida

- Tras proporcionar el enlace oficial, el bot deriva al flujo de [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`) para consultar si el usuario requiere resolver otra inquietud.
- En caso de ejecución independiente, cuenta con respaldo hacia [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
