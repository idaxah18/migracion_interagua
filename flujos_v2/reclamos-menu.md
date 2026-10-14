# Reclamos - Menú

Menú interactivo de selección de tipología de reclamo. Permite al cliente orientar su solicitud técnica o comercial entre averías de agua potable, problemas en redes de alcantarillado sanitario, drenaje pluvial, valores impugnados de facturación o cortes en el suministro.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Contexto de navegación validado con consentimiento LOPD activo.

## Paso 2. Configuración técnica y presentación de opciones

El bot define la URL del servicio SOAP en memoria:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`

Y presenta la lista interactiva con las 5 tipologías de reclamos habilitadas:

> ¿Qué tipo de reclamo deseas realizar?

Opciones de la lista interactiva:
1. **Agua Potable:**  
   Asigna `tipoConsulta: "10"` y transfiere a [_ 5.1 - Fugas de Agua - Configuración](./5-1-fugas-de-agua-configuracion.md).
2. **Aguas Lluvias:**  
   Asigna `tipoConsulta: "null"` y transfiere a [_ 10.1 - Aguas lluvias - Configuración](./10-1-aguas-lluvias-configuracion.md).
3. **Alcantarillado Sanitario:**  
   Asigna `tipoConsulta: "30"` y transfiere a [_ 4.1 - Alcantarillado Sanitario - Configuración](./4-1-alcantarillado-sanitario-configuracion.md).
4. **Reclamos por Facturación:**  
   Transfiere a [_ 6.0 - Alta Facturación - Inicio](./6-0-alta-facturacion-inicio.md).
5. **Afectación del Servicio:**  
   Transfiere a [_ 7.0 - Afectación Servicio - Inicio](./7-0-afectacion-servicio-inicio.md).

Si el usuario escribe *Salir* o cancela la interacción:
- Transfiere a [_ 0.3 - Menú Principal - Bienvenida](./0-3-menu-principal-bienvenida.md).

## Salida

- **Ruta elegida:** Deriva al subflujo de configuración o inicio correspondiente según la categoría seleccionada.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
