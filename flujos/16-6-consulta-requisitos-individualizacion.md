# 16.6 - Consulta Requisitos - Individualización

Informa las condiciones técnicas, requisitos documentales y costos para la individualización de consumos mediante la instalación de medidores independientes por departamento, oficina o unidad funcional en predios compartidos o regímenes de copropiedad.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 6 (*Individualización de Consumo*) desde el menú de requisitos.

## Paso 1. Condiciones técnicas previas y tarifa

El bot envía las consideraciones obligatorias del inmueble:

> *Recuerde:*  
>   
> - El inmueble no debe registrar deuda pendiente ni convenio de pago activo con la empresa.  
> - Para la aceptación del trámite, los medidores deben estar instalados en zonas comunes que faciliten el acceso para la toma de lectura y mantenimiento.  
> - La relación contractual se establecerá mediante la firma del contrato de suscripción del servicio del propietario, copropietarios, administrador o representante legal.  
>   
> *Costo: $6,81 + IVA por cada unidad funcional.*

## Paso 2. Requisitos documentales

El bot detalla los documentos requeridos:

> *Requisitos para individualización*  
>   
> 1. Solicitud del propietario, administrador con nombramiento vigente o representante legal, copia de documento de identificación y/o RUC.  
> 2. En caso de terceros, autorización del propietario, copropietarios, administrador o representante legal, acompañando del poder notarizado emitido en el último año.  
> 3. Certificado de Historia de Dominio emitido en los últimos tres meses: [Registro de la Propiedad](https://www.rpguayaquil.gob.ec/?page_id=26&tknfv=2047b513a-f78c-4e02-9262-737d378d1pj39j9)  
> 4. Cancelación del valor correspondiente a inspección técnica por cada unidad funcional.

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
