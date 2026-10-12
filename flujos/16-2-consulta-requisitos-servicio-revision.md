# 16.2 - Consulta Requisitos - Servicio Revisión

Entrega la información y condiciones técnicas para la contratación del servicio de inspección domiciliaria y control de instalaciones internas, diseñado para detectar fugas ocultas no visibles y evitar el incremento del consumo facturado.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 2 (*Servicio de Revisión y Control de Instalaciones*) desde el menú de requisitos.

## Paso 1. Entrega de infografía y descripción del servicio

El bot envía una imagen informativa oficial acompañada de la explicación del beneficio:

- **Imagen URL:** [https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/11711%2Fimage%2Fe8477b50-73e0-4089-b7d0-64b356016bfa-1760558119834.png](https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/11711%2Fimage%2Fe8477b50-73e0-4089-b7d0-64b356016bfa-1760558119834.png)
- **Texto descriptivo:**
  > Interagua ofrece el servicio de revisión de las instalaciones internas y detección de posibles fugas, para evitar el desperdicio del líquido vital y un incremento en su consumo.

## Salida

- **Finalización de la consulta:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`) o [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
