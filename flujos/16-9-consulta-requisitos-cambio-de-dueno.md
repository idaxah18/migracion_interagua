# 16.9 - Consulta Requisitos - Cambio de Dueño

Detalla los requisitos legales y contractuales para solicitar la actualización de titularidad catastral (cambio de propietario) o el cambio de beneficiario (inquilino o arrendatario) en un contrato de suministro de agua potable.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 9 (*Cambio de Dueño y Usuario*) desde el menú de requisitos.

## Paso 1. Requisitos para cambio de propietario

El bot informa los documentos exigidos al nuevo titular:

> Los requisitos para realizar cambio de propietario son:  
>   
> 1. Certificado de historia de dominio actualizado emitido en los últimos 3 meses: [Registro de la Propiedad](https://www.rpguayaquil.gob.ec/?page_id=26&tknfv=2047b513a-f78c-4e02-9262-737d378d1pj39j9)  
> 2. La relación contractual se establecerá mediante la firma de contrato de suscripción de servicio.  
> 3. Número de contrato de agua potable.  
> 4. Cédula del nuevo propietario.  
> 5. Carta firmada por el/los propietarios.  
> 6. En caso de terceros, carta de autorización o poder notarizado.  
> 7. No tener valores pendientes con la empresa.  
> 8. Para personas jurídicas: RUC, Nombramiento vigente del Representante Legal.

## Paso 2. Requisitos para cambio de beneficiario / arrendatario

El bot expone los requisitos para registrar a un inquilino:

> Los requisitos para realizar cambio de beneficiario son:  
>   
> 1. Solicitud suscrita por el propietario del predio.  
> 2. Contrato de arrendamiento debidamente notarizado.  
> 3. Cédula de identidad de los solicitantes.  
> 4. Para personas jurídicas: RUC, Nombramiento vigente del Representante Legal.  
>   
> ¡Gracias por consultarme, nos veremos en una próxima ocasión!

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
