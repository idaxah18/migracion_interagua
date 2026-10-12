# 16.1 - Consulta Requisitos - Requisitos

Menú interactivo principal del catálogo de requisitos legales, técnicos y administrativos de Interagua. Presenta un listado numerado con los 12 trámites más frecuentes, valida la selección del usuario y lo deriva hacia la guía específica del trámite seleccionado.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento de datos personales (LOPD) verificado positivamente.

## Paso 1. Presentación del catálogo de trámites

El bot genera y despliega el menú de opciones:

> Ingresa el número de la opción que deseas consultar los requisitos  
>   
> 1.- Convenio de pago  
> 2.- Servicio de Revisión y Control de Instalaciones  
> 3.- Control de Efluentes  
> 4.- Certificados Administrativos  
> 5.- Nueva Instalación de Agua Potable o Alcantarillado Sanitario  
> 6.- Individualización de Consumo  
> 7.- Certificado de Aprobación del Régimen de Propiedad Horizontal  
> 8.- Certificado de Mejoras de Alcantarillado o Inspección final Municipal  
> 9.- Cambio de Dueño y Usuario  
> 10.- Exoneraciones y Descuentos por Ley  
> 11.- Cierre de Servicio de Agua Potable  
> 12.- Carta Modelo de Pruebas de Descargo  
>   
> Ingresa el número de la opción requerida.   
>   
> Ingresa 0 para salir del menú.

## Paso 2. Evaluación de la selección

El usuario ingresa el número por teclado:
- Si ingresa un número fuera del rango 0 al 12:  
  > La opción que ingresaste es incorrecta.   
  > Por favor, inténtalo nuevamente.  
  *(Vuelve a solicitar el número).*
- Si ingresa `0`: Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- Si ingresa del `1` al `12`: Guarda en memoria `itemRequisito` y transfiere al flujo respectivo.

## Salida (Enrutamiento por opción)

1. **Opción 1:** Transfiere a [_ 2.0 - Convenio de Pago - Inicio](./2-0-convenio-de-pago-inicio.md) (Skill ID `51052`).
2. **Opción 2:** Transfiere a [_ 16.2 - Consulta Requisitos - Servicio Revisión](./16-2-consulta-requisitos-servicio-revision.md) (Skill ID `51175`).
3. **Opción 3:** Transfiere a [_ 16.3 - Consulta Requisitos - Control Efluentes](./16-3-consulta-requisitos-control-efluentes.md) (Skill ID `51174`).
4. **Opción 4:** Transfiere a [_ 16.4 - Consulta Requisitos - Certificados Administrativos](./16-4-consulta-requisitos-cert-admin.md) (Skill ID `51173`).
5. **Opción 5:** Transfiere a [_ 16.5 - Consulta Requisitos - Nueva Instalación](./16-5-consulta-requisitos-nueva-instalacion.md) (Skill ID `51172`).
6. **Opción 6:** Transfiere a [_ 16.6 - Consulta Requisitos - Individualización](./16-6-consulta-requisitos-individualizacion.md) (Skill ID `51171`).
7. **Opción 7:** Transfiere a [_ 16.7 - Consulta Requisitos - Certificado Aprobación](./16-7-consulta-requisitos-cert-aprobacion.md) (Skill ID `51170`).
8. **Opción 8:** Transfiere a [_ 16.8 - Consulta Requisitos - Certificado Mejoras](./16-8-consulta-requisitos-cert-mejora.md) (Skill ID `51169`).
9. **Opción 9:** Transfiere a [_ 16.9 - Consulta Requisitos - Cambio de Dueño](./16-9-consulta-requisitos-cambio-de-dueno.md) (Skill ID `51168`).
10. **Opción 10:** Transfiere a [_ 16.10 - Consulta Requisitos - Exoneraciones](./16-10-consulta-requisitos-exoneraciones.md) (Skill ID `51165`).
11. **Opción 11:** Transfiere a [_ 16.11 - Consulta Requisitos - Cierre Servicio](./16-11-consulta-requisitos-cierre-servicio.md) (Skill ID `51166`).
12. **Opción 12:** Transfiere a [_ 16.12 - Consulta Requisitos - Carta Modelo](./16-12-consulta-requisitos-carta-modelo.md) (Skill ID `51167`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
