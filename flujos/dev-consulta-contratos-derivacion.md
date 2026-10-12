# Consulta de contratos - Derivación (Entorno de Desarrollo)

Este flujo es el homólogo en el entorno de pruebas de [Consulta de contratos - Derivación](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/consulta-contratos-derivacion.md). Actúa como la centralita de enrutamiento tras la identificación exitosa de un contrato en desarrollo, dirigiendo al usuario hacia las versiones de prueba correspondientes de cada trámite.

A diferencia del flujo de producción (donde la conexión de inicio se encontraba aislada), esta versión de desarrollo cuenta con la conexión directa desde el nodo de inicio hacia el nodo condicional de enrutamiento.

## Datos que debe traer al entrar

- `contexto`: Identificador del trámite que originó la consulta.
- `numeroContrato`: Número de contrato confirmado del cliente.
- `identificacion`: Cédula o RUC asociado.
- `datos_cliente`: Objeto en memoria con los datos del contrato de prueba.

## Enrutamiento según el trámite solicitado

El flujo evalúa el valor contenido en la variable `contexto` y transfiere la sesión a la pantalla correspondiente:

1. **Consulta de saldo de consumo:**
   - Si `contexto` es igual a `saldos`:
   - Transfiere a [1.1 - Saldos Consumo Agua - Confirmación](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/1-1-saldos-consumo-agua-confirmacion.md) (Skill `51070`).

2. **Convenio de pago:**
   - Si `contexto` es igual a `convenio_pago`:
   - Transfiere a [2.2 - Convenio de Pago - Porcentaje y Plazo](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/2-2-convenio-de-pago-porcentaje-y-plazo.md) (Skill `51094`).

3. **Reclamos técnicos e incidencias operativas:**
   - Si `contexto` coincide con cualquiera de los siguientes motivos:
     - `fuga_agua` (Fuga de agua potable)
     - `alcantarillado_sanitario` (Rebose de aguas servidas)
     - `aguas_lluvia` (Colapso de aguas lluvias)
     - `faltante_tapa` (Faltante de tapa de alcantarilla)
   - Transfiere a [Reclamo Técnico - Información](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/reclamo-tecnico-informacion.md) (Skill `51105`).

4. **Reclamo por alta facturación:**
   - Si `contexto` es igual a `alta_facturacion`:
   - Transfiere a [6.2 - Alta Facturación - Facturas](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/6-2-alta-facturacion-facturas.md) (Skill `51098`).

5. **Afectación o cortes del servicio:**
   - Si `contexto` es igual a `afectacion_servicio`:
   - Transfiere a [7.2 - Afectación Servicio - Interrupciones](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/7-2-afectacion-servicio-interrupciones.md) (Skill `51107`).

6. **Venta de servicios comerciales (Versión Dev):**
   - Si `contexto` es igual a `venta_servicios`:
   - Transfiere a [Dev 12.1 - Venta Servicios - Servicios](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/dev-12-1-venta-servicios-servicios.md) (Skill `51900`).

7. **Certificados administrativos:**
   - Si `contexto` es igual a `certificados_administrativos`:
   - Transfiere a [13.1 - Certificados - Precio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/13-1-certificados-precio.md) (Skill `51086`).

## Salida de respaldo

Si no se reconoce el contexto o la sesión se inicia sin parámetros:
- El flujo retorna de forma segura a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`).
