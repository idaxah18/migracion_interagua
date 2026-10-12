# Campaña de solicitud de limpieza (Pauta solicitar limpieza)

Este flujo actúa como punto de aterrizaje para clientes que ingresan desde campañas publicitarias o enlaces patrocinados en redes sociales ("Pautas") orientados a la solicitud de servicios de limpieza de pozos sépticos, desobstrucción de redes o mantenimiento de trampas de grasa.

Informa sobre la autorización legal de protección de datos personales y conecta de manera prioritaria al usuario con un asesor humano para coordinar la cotización e inspección del servicio en terreno.

## Datos que debe traer al entrar

- Enlace de campaña publicitaria o referencia de pauta digital.

## Paso 1. Notificación de política de privacidad

Al ingresar desde la pauta externa, el bot informa al ciudadano las condiciones de tratamiento de sus datos:

> Al continuar conectado usted autoriza el uso de sus datos personales a Interagua y sus empresas relacionadas. 
> 
> Usted puede consultar la política de privacidad en el siguiente link: bit.ly/PPInteragua

El usuario responde o confirma la interacción para proceder con la asignación.

## Paso 2. Asignación y conexión con un asesor comercial

El sistema activa la transferencia a la cola de atención preferencial de operadores (`JelouConnect`):

- **Al asignarse un operador con éxito:**
  El bot emite la confirmación de atención:
  > 👷 En unos momentos un asesor se comunicará contigo para dar seguimiento a tu solicitud.
  Y el caso queda abierto en la consola del agente de ventas para coordinar los detalles del servicio de limpieza.

## Salidas del flujo

- **Atención humana:** La conversación continúa con el operador asignado.
- **Salida de respaldo:** En caso de ejecución aislada sin derivación, cuenta con retorno a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
