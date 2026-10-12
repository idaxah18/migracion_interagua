# 16.4 - Consulta Requisitos - Certificados Administrativos

Orienta al usuario sobre la descarga gratuita de facturas a través de canales digitales 24/7 y detalla los requisitos y costos administrativos para la emisión física de certificaciones de deuda, historial de pagos y duplicados de facturas.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 4 (*Certificados Administrativos*) desde el menú de requisitos.

## Paso 1. Información de obtención gratuita digital

El bot informa los canales sin costo para consultar y obtener comprobantes:

> Recuerde que usted puede obtener su factura directamente y SIN COSTO, en nuestros canales virtuales disponibles 24/7:   
> *Portal web:* [https://www.interagua.com.ec/](https://www.interagua.com.ec/)  
>   
> *Aplicación móvil:*  
> - [Google Play](https://play.google.com/store/apps/details?id=com.veolia.interagua)  
> - [App Store](https://apps.apple.com/ec/app/agencia-virtual-interagua/id1500970475)  
>   
> *Chateando con IRIS al número 098 501 4693:* [https://wa.me/593985014693](https://wa.me/593985014693)  
>   
> En cumplimiento con las normativas del Servicio de Rentas Internas, le recomendamos registrar su consentimiento para recibir su factura por medio electrónico. Sus facturas son deducibles de sus gastos personales.

## Paso 2. Requisitos y costos para certificaciones físicas

El bot detalla los requisitos para trámites presenciales o formales:

> 1. Información sobre deudas, pagos, estados de cuenta, deben ser solicitados exclusivamente por el propietario.  
> 2. Carta de solicitud, requiriendo información sobre deuda, pagos, suscripción del servicio.  
> 3. Número de contrato.  
>   
> *Valor de certificación: USD$1,29 + IVA.*  
>   
> *Duplicado de facturas:*  
> 1. Número de contrato.  
> 2. En caso de terceros, carta de autorización del propietario.  
>   
> *Costo de copia de facturas: $1.29 + IVA.*

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
