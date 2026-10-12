# 16.12 - Consulta Requisitos - Carta Modelo

Guía al cliente sobre el derecho a presentar pruebas de descargo en caso de notificaciones por presunto fraude o conexiones no autorizadas, explica los plazos legales (5 días), entrega el formulario digital de radicación y suministra el documento oficial editable de la Carta Modelo de Descargo.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 12 (*Carta Modelo de Pruebas de Descargo*) desde el menú de requisitos.

## Paso 1. Explicación del derecho a descargo y canales

El bot envía las directrices legales:

> *Carta modelo de pruebas de descargo*  
>   
> - Cuando se haya encontrado un posible fraude en su predio, su contrato ingresará en un proceso de investigación.  
> - Usted tiene el derecho de presentar una prueba de descargo con el fin de explicar que usted no es responsable del fraude encontrado.  
> - Disponemos de varios medios para que usted pueda registrar su prueba de descargo (en un plazo de 5 días):  
>   
> 1. Si usted desea registrar virtualmente su formulario de pruebas de descargo, puede realizarlo a través del botón que se encuentra más adelante.  
> 2. Si usted desea puede enviar un correo electrónico a la cuenta de atencionalcliente@interagua.com.ec adjuntando su prueba de descargo.  
> 3. Si usted desea puede acercarse a una de nuestras oficinas de atención al cliente para registrar presencialmente su prueba de descargo: [Citas Previas Interagua](https://citasprevias.interagua.com.ec/)  
>   
> *Este trámite no tiene costo.*  
>   
> Te presento nuestra carta de modelo de pruebas ⬇  
> [Carta Modelo Web](https://bit.ly/3594Su6)  
>   
> Formulario en línea:  
> [Formulario de Descargo Google](https://docs.google.com/forms/d/e/1FAIpQLSd-24PIWHNQhipSIORVJ9q0_pEqeIoP5xyVbx-isw0htptpeg/viewform)

## Paso 2. Entrega del archivo digital adjunto

El bot despacha directamente el archivo adjunto:

- **Nombre del archivo:** Carta Modelo de Prueba
- **URL de descarga:** [https://s3-us-west-2.amazonaws.com/cdn.devlabs.tech/interagua-gea-documents/Carta_modelo_de_pruebas](https://s3-us-west-2.amazonaws.com/cdn.devlabs.tech/interagua-gea-documents/Carta_modelo_de_pruebas)

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
