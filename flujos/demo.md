# Demostración del Asistente Virtual IRIS (Demo)

Este flujo se utiliza con fines de prueba y demostración interactiva de las nuevas capacidades webview integradas en WhatsApp para **IRIS**, el asistente virtual de Interagua. Permite comprobar cómo se despliegan las ventanas emergentes web dentro del chat para trámites complejos o formularios enriquecidos de autoservicio.

## Datos que debe traer al entrar

- No requiere parámetros de entrada obligatorios. Diseñado como flujo piloto de demostración de interfaz gráfica.

## Mensaje al cliente

El bot presenta la tarjeta interactiva de bienvenida con apertura de WebView:

> Soy IRIS, asistente virtual de Interagua.
> 
> Puedes continuar con tu gestión, desde nuestra nueva opción. Allí podrás realizar tu solicitud de forma rápida y segura.

### Características del componente WebView:
- Al pulsar el botón de la tarjeta interactiva, WhatsApp abre una ventana integrada segura del navegador sin sacar al usuario de la aplicación.
- Permite cargar formularios web responsivos para ingreso ágil de datos, carga de fotos o documentos de respaldo.

## Salida

- El flujo mantiene su derivación base hacia [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`) para reiniciar la atención del usuario una vez finalizada la prueba.
