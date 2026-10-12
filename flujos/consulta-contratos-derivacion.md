# Consulta de contratos - Derivación

Este flujo actúa como una centralita inteligente de enrutamiento tras la consulta exitosa de un contrato. Una vez que el cliente seleccionó o ingresó el contrato con el que desea realizar su trámite en el flujo de consulta previa, este componente evalúa la intención original del usuario y lo conduce de manera directa y transparente al paso correspondiente de su gestión.

No muestra mensajes de texto ni menús visuales al cliente, garantizando una transición inmediata y fluida entre la identificación de la cuenta y la resolución del trámite.

## Datos que debe traer al entrar

- `contexto`: Identificador del trámite o gestión en curso que solicitó la consulta del contrato.
- `numeroContrato`: Número de contrato confirmado y validado del cliente.
- `identificacion`: Número de cédula o RUC asociado a la cuenta.
- `datos_cliente` o `Contratos`: Objeto con la información técnica y comercial de la cuenta recuperada.

## Enrutamiento según el trámite solicitado

El flujo evalúa el valor contenido en la variable de memoria `contexto` y transfiere al usuario a la pantalla correspondiente:

1. **Consulta de saldo de consumo de agua:**
   - Si `contexto` es igual a `saldos`:
   - Se deriva al flujo [1.1 - Saldos Consumo Agua - Confirmación](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/1-1-saldos-consumo-agua-confirmacion.md) (Skill `51070`), donde se le presenta el desglose de sus valores pendientes y fecha límite de pago.

2. **Convenio de pago:**
   - Si `contexto` es igual a `convenio_pago`:
   - Se transfiere al flujo [2.2 - Convenio de Pago - Porcentaje y Plazo](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/2-2-convenio-de-pago-porcentaje-y-plazo.md) (Skill `51094`), donde podrá simular su cuota inicial y meses de financiamiento.

3. **Reclamos técnicos e incidencias de agua o alcantarillado:**
   - Si `contexto` coincide con cualquiera de los siguientes motivos:
     - `fuga_agua` (Fuga de agua potable en acera o calzada)
     - `alcantarillado_sanitario` (Rebose o taponamiento de aguas servidas)
     - `aguas_lluvia` (Inundación o colapso de sumideros pluviales)
     - `faltante_tapa` (Peligro por caja o alcantarilla sin tapa)
   - Se transfiere a [Reclamo Técnico - Información](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/reclamo-tecnico-informacion.md) (Skill `51105`) para recopilar la dirección exacta, referencias y fotos del incidente.

4. **Reclamo por alta facturación:**
   - Si `contexto` es igual a `alta_facturacion`:
   - Se transfiere al flujo [6.2 - Alta Facturación - Facturas](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/6-2-alta-facturacion-facturas.md) (Skill `51098`), donde se despliegan las planillas emitidas para elegir la factura objeto del reclamo.

5. **Afectación o corte del servicio de agua potable:**
   - Si `contexto` es igual a `afectacion_servicio`:
   - Se transfiere a [7.2 - Afectación Servicio - Interrupciones](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/7-2-afectacion-servicio-interrupciones.md) (Skill `51107`), donde se consulta si el sector tiene trabajos programados o cortes imprevistos en ejecución.

6. **Venta de servicios comerciales e ingeniería:**
   - Si `contexto` es igual a `venta_servicios`:
   - Se deriva al flujo [12.1 - Venta Servicios - Servicios](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/12-1-venta-servicios-servicios.md) (Skill `51078`) para seleccionar el catálogo de servicios (limpieza de pozo séptico, desinfección, etc.).

7. **Certificados administrativos:**
   - Si `contexto` es igual a `certificados_administrativos`:
   - Se transfiere a [13.1 - Certificados - Precio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/13-1-certificados-precio.md) (Skill `51086`) para cotizar y solicitar el certificado de no adeudar o historial de pagos.

## Salida por defecto y excepciones

Si la variable `contexto` no coincide con ninguno de los trámites anteriores, o si se produce un salto directo sin parámetro:
- El flujo redirige de forma segura a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`) para reiniciar la atención del cliente sin dejar la conversación bloqueada.
