# Opción no disponible (Utility)

Este flujo de utilidad actúa como un controlador de seguridad y obsolescencia. Se invoca cuando un usuario intenta acceder a una funcionalidad temporalmente desactivada, un enlace caducado de un mensaje previo o una opción de menú que ha sido retirada del catálogo de servicios activos de Interagua.

Su propósito es comunicar la indisponibilidad de manera amable y retornar al cliente de inmediato al menú de bienvenida para que pueda elegir entre los servicios vigentes.

## Datos que debe traer al entrar

- No requiere parámetros de entrada obligatorios. Se invoca de manera directa desde opciones o ramas obsoletas.

## Mensaje al cliente

El bot presenta un mensaje informativo conciso:

> Esta opción ya no se encuentra disponible.

## Salida

Inmediatamente después de notificar al usuario:
- El flujo lo reconduce de forma automática a [0.3 - Menú Principal - Bienvenida](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-3-menu-principal-bienvenida.md) (Skill `51161`) para desplegar nuevamente el menú de bienvenida y los servicios activos de la empresa.
- En caso de acceso independiente o desconexión, cuenta con salida de respaldo a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
