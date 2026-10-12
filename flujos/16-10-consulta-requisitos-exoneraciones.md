# 16.10 - Consulta Requisitos - Exoneraciones

Permite al cliente consultar los requisitos y condiciones reglamentarias para acceder a los beneficios y descuentos por ley en las tarifas de agua potable y saneamiento (Ley Orgánica del Adulto Mayor y Ley Orgánica de Discapacidad).

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 10 (*Exoneraciones y Descuentos por Ley*) desde el menú de requisitos.

## Paso 1. Selección del tipo de beneficio

El bot presenta los botones de selección:

> ¿Qué tipo de exoneración deseas consultar? 🤔

Opciones con botones:
1. **Tercera edad:** Muestra los requisitos de la Ley Orgánica del Adulto Mayor (Paso 2A).
2. **Discapacidad:** Muestra los requisitos de la Ley Orgánica de Discapacidad (Paso 2B).

## Paso 2A. Descuento por Ley Orgánica del Adulto Mayor

El bot detalla los requisitos y alcance del beneficio:

> *Descuento por Ley Orgánica del Adulto Mayor*  
>   
> Los requisitos son los siguientes:  
> 1. Presentar la cédula de identidad o pasaporte del titular de la conexión de agua potable.  
> 2. Ser mayor de 65 años.   
> 3. Mantener contrato de suministro de agua potable.  
> 4. En caso de que el adulto mayor solicitante sea usufructuario del predio, deberá presentar Certificado de Historia de Dominio actualizada en los últimos tres meses.  
> 5. En caso de aprobación se exonera el 50% del valor del consumo por agua potable hasta 34m3. En caso de que el consumo exceda 34m3 por este concepto, se facturará dicho excedente en base a la tarifa normal del servicio.

## Paso 2B. Descuento por Ley Orgánica de Discapacidad

El bot informa las condiciones y porcentaje de subsidio:

> *Descuento por Ley Orgánica de Discapacidad*  
>   
> Los requisitos son los siguientes:  
> • Presentar la cédula de ciudadanía del solicitante, en la que conste el porcentaje de discapacidad, el cual debe ser igual o superior al 30%.  
> • Mantener un contrato de suministro de agua potable, suscrito por el solicitante o por una persona calificada como sustituta directa o cuidadora.  
> • En el caso de personas jurídicas sin fines de lucro, presentar la acreditación emitida por la autoridad competente.  
> • Si el solicitante es inquilino, deberá presentar el contrato de arrendamiento debidamente notarizado.  
>   
> *Beneficio:*  
> Se exonera el 50% del valor del servicio de agua potable y alcantarillado sanitario.

## Paso 3. Advertencia legal sobre auditorías

El bot recuerda el régimen sancionatorio por uso indebido:

> *IMPORTANTE:* La empresa podrá verificar la información o inspeccionar en sitio en cualquier momento, la correcta aplicación del subsidio.  
>   
> El titular y beneficiario del servicio es responsable de la actualización de los datos.   
>   
> En caso de determinarse la aplicación indebida de un beneficio la Empresa cesará la aplicación del mismo y podrá realizar un cargo a partir de la fecha en que se detecte.

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
