# 8.1 - Descargar Facturas - Enlace

Entrega al usuario los canales oficiales y enlaces directos de descarga para consultar y obtener sus facturas electrónicas de Interagua, tanto en el portal web (Stupendo) como a través de la aplicación móvil "Agencia Virtual".

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Autorización LOPD: Verificada.

## Paso 2. Entrega de enlaces de descarga (Call to Action)

El asistente envía los mensajes interactivos con botones de enlace directo:

1. **Opción Web (Portal Stupendo):**
   > Te invito a que verifiques tu planilla registrándote en nuestra pagina web.  
   > 
   > _*El registro debe realizarlo el beneficiario del contrato.*_  
   - Botón interactivo: **Pagina Web**  
   - Enlace: `https://app.stupendo.ec/login/user`

2. **Opción Móvil (Interagua Agencia Virtual):**
   > Tambien puedes descargar nuestra app para SmartPhone Interagua Agencia Virtual, totalmente gratis desde tu tienda favorita.  
   - Botón interactivo: **Descargar APP**  
   - Enlace: `https://onelink.to/jeq9qw`

3. **Mensaje de cierre:**
   > Te invito a que verifiques tu planilla registrandote en nuestra pagina web o descargando nuestra app para SmartPhone Interagua Agencia Virtual, totalmente gratis desde tu tienda favorita.  
   > 
   > Gracias por consultarme.

## Salida

Una vez entregados los accesos:

- Transfiere a: [_ Atención Adicional](./atencion-adicional.md) para consultar si el usuario requiere alguna otra gestión antes de cerrar la sesión.
- En caso de cancelación: Transfiere a [_ 0 - Inicio](./0-inicio.md).
