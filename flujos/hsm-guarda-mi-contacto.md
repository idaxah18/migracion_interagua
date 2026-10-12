# Tarjeta de contacto oficial (HSM - Guarda mi contacto)

Este flujo se utiliza en campañas de mensajería masiva (plantillas HSM) o cuando un usuario inicia contacto por primera vez. Su objetivo principal es remitir al cliente la tarjeta digital oficial de contacto (vCard de WhatsApp) de Interagua para que la agregue a la agenda telefónica de su dispositivo móvil.

Guardar el contacto oficial permite que WhatsApp habilite sin restricciones la recepción de botones interactivos, listas desplegables, videos y archivos multimedia, garantizando la mejor experiencia de servicio al cliente.

## Datos que debe traer al entrar

- No requiere datos de entrada obligatorios. Se dispara como plantilla HSM o respuesta automática de bienvenida.

## Contenido de la interacción

El sistema despacha un mensaje nativo de tipo tarjeta de contacto (`contacts`) con los datos institucionales de Interagua:

- **Nombre mostrado:** Interagua (Canal Oficial de WhatsApp)
- **Número telefónico institucional:** +593 98 318 8929 (o número oficial de atención de la concesionaria)
- **Organización:** Empresa de Agua Potable y Alcantarillado de Guayaquil - Concesionaria Interagua

El cliente solo debe presionar el botón de WhatsApp **"Guardar contacto"** o **"Añadir a contactos"** en su teléfono para registrar el número de manera instantánea.

## Salida

- Tras el envío de la tarjeta de contacto, el flujo deriva a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`) para que el usuario pueda iniciar cualquier consulta o trámite en el canal.
