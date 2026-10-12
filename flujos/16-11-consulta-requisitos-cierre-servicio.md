# 16.11 - Consulta Requisitos - Cierre Servicio

Informa los requisitos, costos y procedimientos legales para solicitar la suspensión temporal o la cancelación definitiva de la conexión de agua potable en predios deshabitados o inmuebles en demolición/remodelación.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 11 (*Cierre de Servicio de Agua Potable*) desde el menú de requisitos.

## Paso 1. Selección de la modalidad de cierre

El bot presenta los botones de selección:

> ¿Deseas realizar un cierre temporal o uno definitivo?

Opciones con botones:
1. **Cierre temporal:** Muestra los requisitos y tarifas de suspensión por desocupación (Paso 2A).
2. **Cierre definitivo:** Muestra los requisitos y tarifas de levantamiento definitivo de conexión (Paso 2B).

## Paso 2A. Requisitos para cierre temporal

El bot detalla las condiciones para suspender el servicio:

> *Cierre temporal*  
>   
> Si su predio está deshabitado o no requerirá del servicio por un tiempo mayor a 3 meses, le recomendamos que solicite un cierre temporal.   
>   
> Los requisitos son los siguientes:  
> 1. Carta de solicitud firmada por el/los propietarios.  
> 2. Copia de cédula.   
> 3. Para personas jurídicas: RUC, Nombramiento vigente del Representante Legal.   
> 4. Importante no mantener valores pendientes de pago.   
>   
> *Suspensión o cierre temporal del servicio de AAPP guía de 1/2 ”hasta 1“ costo: $6,81 + IVA.*  
>   
> Para guías de mayor diámetro se emitirá cotización particular.

## Paso 2B. Requisitos para cierre definitivo

El bot expone los requisitos legales para la anulación de la toma:

> *Cierre definitivo*  
>   
> Este trámite debe ser presentado exclusivamente por el propietario del predio, para personas jurídicas debe adjuntar el RUC, nombramiento inscrito y vigente del representante legal.  
>   
> Los requisitos son los siguientes:  
> 1. Certificado de historia de dominio emitido en los últimos 3 meses: [Registro de la Propiedad](https://www.rpguayaquil.gob.ec/?page_id=26&tknfv=2047b513a-f78c-4e02-9262-737d378d1pj39j9)  
> 2. Cédula de el/los propietarios.  
> 3. En caso de terceros, carta de autorización y poder notarizado.  
> 4. No debe registrarse deuda pendiente por la Empresa.  
> 5. Debe cancelar el valor por el servicio, que varía de acuerdo a su conexión.  
>   
> *Cierre definitivo del servicio de AAPP guía de 1/2 ”hasta 1” $103.56 + IVA.*  
>   
> Para guías de mayor diámetro se emitirá cotización particular.

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
