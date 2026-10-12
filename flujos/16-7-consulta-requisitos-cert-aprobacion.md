# 16.7 - Consulta Requisitos - Certificado Aprobación

Expone los requisitos técnicos, memorias y juegos de planos necesarios para tramitar el Certificado de Aprobación de Sistemas Sanitarios en Régimen de Propiedad Horizontal, desglosando la tabla de costos según el número de unidades funcionales.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 7 (*Certificado de Aprobación del Régimen de Propiedad Horizontal*) desde el menú de requisitos.

## Paso 1. Requisitos técnicos y planos

El bot detalla la documentación arquitectónica y sanitaria exigida:

> Los requisitos para obtener un certificado de aprobación son:  
>   
> 1. Carta de solicitud del propietario o autorizado.  
> 2. Memorias técnicas/administrativas.  
> 3. 2 juegos de planos sanitarios y 2 juegos de planos arquitectónicos. Para el caso de edificaciones con más de 3 años de construcción, ambos juegos podrán ser físicos.  
> 4. Para edificaciones nuevas es obligatorio la presentación de plano físico y digital.  
>   
> *Nota:* Para nuevos proyectos debe contar con aprobación de factibilidad del servicio.  
>   
> *Tiempo de atención:* 15 días a partir del pago por el servicio.

## Paso 2. Tabla de tarifas por unidades funcionales

El bot informa la escala de costos aplicable:

> *Para trámites de Propiedad Horizontal, de acuerdo al número de departamentos, oficinas y locales comerciales:*  
>   
> Costo (USD):  
> a) Hasta 4 unidades funcionales: $15.67  
> b) Entre 5 y 10 unidades funcionales: $32.70  
> c) Entre 11 y 20 unidades funcionales: $44.28  
> d) Entre 21 y 30 unidades funcionales: $54.50  
> e) Entre 31 y 50 unidades funcionales: $76.99  
> f) Desde 51 unidades funcionales en adelante: $87.89  
>   
> *A todos estos valores se les debe adicionar el IVA.*

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
