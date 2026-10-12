# 16.5 - Consulta Requisitos - Nueva Instalación

Detalla los requisitos legales, técnicos y contractuales para solicitar la instalación de un nuevo medidor y conexión a las redes matrices de agua potable o alcantarillado sanitario, indicando plazos de atención y procedimiento de presupuesto.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 5 (*Nueva Instalación de Agua Potable o Alcantarillado Sanitario*) desde el menú de requisitos.

## Paso 1. Entrega de requisitos para nueva instalación

El bot despliega los requisitos normativos:

> *Para solicitar un medidor debes cumplir con los siguientes requisitos:*  
>   
> 1. Presentación de la cédula del propietario y para personas jurídicas, RUC, copia del nombramiento inscrito y vigente del representante legal.   
> 2. Certificado de Historia de Dominio emitido en los últimos tres meses: [Registro de la Propiedad](https://www.rpguayaquil.gob.ec/?page_id=26&tknfv=2047b513a-f78c-4e02-9262-737d378d1pj39j9)  
> 3. Para casos de Parroquias Rurales, debe presentar el Certificado de posesión emitido por el GAD al cual pertenezca, aplica solo si es el propietario del predio.   
> 4. El solicitante no deberá mantener deuda pendiente con la empresa.   
> 5. La relación contractual se establece mediante norma de contrato de suscripción de servicio.   
> 6. En caso de terceros, autorización del propietario, copropietarios, administrador o representante legal, acompañando del poder notarizado emitido en el último año.   
>   
> *Valor:* Determinado el presupuesto referencial de la obra, el cliente deberá formalizar su aceptación, previo al inicio de la misma.   
>   
> *Tiempo de atención:* 15 días luego de cancelado el valor de instalación o anticipo de obra.

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
