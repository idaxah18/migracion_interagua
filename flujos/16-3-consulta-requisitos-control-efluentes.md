# 16.3 - Consulta Requisitos - Control Efluentes

Entrega los requisitos y accesos digitales para la obtención del Certificado de Aprobación de Sistemas de Tratamiento y control de descargas de aguas residuales industriales y comerciales (trampas de grasa y sedimentadores), incluyendo enlaces a formularios oficiales y guías técnicas de descarga.

## Datos que debe traer al entrar

- `contexto`: `consulta_requisitos`.
- Teléfono del usuario: `{{$user.phone}}`.
- Selección de la opción 3 (*Control de Efluentes*) desde el menú de requisitos.

## Paso 1. Entrega de requisitos obligatorios para trampa de grasa

El bot despliega la lista de documentación requerida:

> *REQUISITOS TRAMPA DE GRASA*  
>   
> 1. Certificado del Establecimiento Registrado (SRI).  
> 2. Carta de autorización del dueño del contrato de agua potable (Aplica para terceros).  
> 3. Copia de la cédula de identidad.

## Paso 2. Entrega de formularios y guías técnicas en línea

El bot suministra los enlaces a los formularios de registro y documentación oficial:

1. **Formulario de registro de usuarios industriales:**  
   [https://docs.google.com/forms/d/e/1FAIpQLSdQ7x3CrYyLT_tqr1Y2YPNPR9CIbKzaVPJF6YSng9QgETD9Dg/viewform?urp=gmail_link](https://docs.google.com/forms/d/e/1FAIpQLSdQ7x3CrYyLT_tqr1Y2YPNPR9CIbKzaVPJF6YSng9QgETD9Dg/viewform?urp=gmail_link)

2. **Formulario de registro de establecimientos comerciales:**  
   [https://docs.google.com/forms/d/1UAsBexdlx18KkNeIUlbjItS3o6k0CbehlSMIoXi6s-Y](https://docs.google.com/forms/d/1UAsBexdlx18KkNeIUlbjItS3o6k0CbehlSMIoXi6s-Y)

3. **Guía de procedimiento para aprobación de sistemas de tratamiento:**  
   [https://www.interagua.com.ec/sites/default/files/2023-03/guia_del_procedimiento_para_la_obtencion_del_certificado_de_aprobacion_de_los_sistemas_de_tratamiento.docx.pdf](https://www.interagua.com.ec/sites/default/files/2023-03/guia_del_procedimiento_para_la_obtencion_del_certificado_de_aprobacion_de_los_sistemas_de_tratamiento.docx.pdf)

4. **Guía de mantenimiento y limpieza de trampas de grasa:**  
   [https://www.interagua.com.ec/sites/default/files/2023-03/gi-opc-003_guia_usuario_mantenimiento_limpieza_sistema_sedimentador_trampa_grasav3.pdf](https://www.interagua.com.ec/sites/default/files/2023-03/gi-opc-003_guia_usuario_mantenimiento_limpieza_sistema_sedimentador_trampa_grasav3.pdf)

5. **Listado de actividades y usuarios obligados:**  
   [https://www.interagua.com.ec/sites/default/files/2023-03/certif._tg_web_1.pdf](https://www.interagua.com.ec/sites/default/files/2023-03/certif._tg_web_1.pdf)

## Salida

- **Finalización de la consulta:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
