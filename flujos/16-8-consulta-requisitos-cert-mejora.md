# 16.8 - Consulta Requisitos - Certificado Mejoras

Informa los requisitos exigidos para la obtención del Certificado de Mejoras de Alcantarillado o Inspección Final Municipal, diferenciando los documentos solicitados para empresas (personas jurídicas) y personas naturales.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 8 (*Certificado de Mejoras de Alcantarillado o Inspección final Municipal*) desde el menú de requisitos.

## Paso 1. Requisitos para empresas y personas naturales

El bot presenta la lista de requisitos diferenciada:

> Para obtener un Certificado de Mejoras de Alcantarillado o Inspección Final Municipal debe tener los siguientes requerimientos:  
>   
> *En caso de empresas:*  
> 1. Registro de construcción actualizado año vigente.  
> 2. RUC de la empresa.  
> 3. Nombramiento del Representante Legal.  
> 4. En caso de tercero o ser el Responsable Técnico: Carta de Autorización del Dueño.  
> 5. Copia de cédula de responsable técnico.  
> 6. Copia de cédula del propietario.  
> 7. Copia de cédula del autorizado.  
>   
> *En caso de Persona Natural:*  
> 1. Registro de construcción actualizado año vigente.  
> 2. En caso de tercero o ser el Responsable Técnico: Carta de Autorización del Dueño.  
> 3. Copia de cédula de responsable técnico.  
> 4. Copia de cédula del propietario.  
> 5. Copia de cédula del autorizado.

## Paso 2. Consideraciones técnicas de saneamiento

El bot recuerda las condiciones complementarias de inspección:

> *Recuerda:*  
> - En caso de que la actividad lo requiera se deberá presentar un certificado de trampa de grasas vigente (locales que elaboren alimentos/bebidas, lubricadoras/lavadoras).  
> - Es importante saber cuántas cajas de aguas servidas (AASS) existen y de qué material es la tapa.

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
