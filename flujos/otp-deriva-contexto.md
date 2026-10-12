# Validación de código de seguridad (OTP) - Derivación

Este flujo se ejecuta inmediatamente después de que el usuario ha verificado de manera exitosa su identidad ingresando el código OTP de 6 dígitos. Su única responsabilidad consiste en inspeccionar el contexto del trámite y el tipo de servicio contratado para retornar al usuario a la fase de formalización o registro del trámite correspondiente.

No muestra mensajes visuales al cliente, actuando como un puente invisible y seguro en la conversación.

## Datos que debe traer al entrar

- `contexto`: Identificador del trámite en curso (`venta_servicios` o `certificados_administrativos`).
- `is_trampa_grasa`: Variable booleana (`true` o `false`) que indica si el servicio solicitado corresponde a un certificado de trampa de grasa.
- `numeroContrato`: Número de contrato validado.
- `identificacion`: Documento de identidad del titular.
- `correo_selected`: Correo electrónico confirmado donde se validó el código de seguridad.

## Enrutamiento según el contexto verificado

El flujo comprueba las variables en memoria y transfiere al usuario a la pantalla correspondiente:

1. **Venta de servicios comerciales e ingeniería:**
   - Si `contexto` es igual a `venta_servicios`:
     - **Si `is_trampa_grasa` es `true`:**
       Se transfiere al flujo [12.4 - Venta Servicios - Trampa Grasa](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/12-4-venta-servicios-trampa-grasa.md) (Skill `51082`), para completar la información técnica del establecimiento comercial y solicitar la inspección de trampa de grasa.
     - **Si `is_trampa_grasa` es `false`:**
       Se transfiere al flujo [12.3 - Venta Servicios - Registrar](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/12-3-venta-servicios-registrar.md) (Skill `51081`), donde se formaliza y guarda la orden de trabajo del servicio solicitado.

2. **Certificados administrativos:**
   - Si `contexto` es igual a `certificados_administrativos`:
     - Se transfiere al flujo [13.2 - Certificados - Confirmación](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/13-2-certificados-confirmacion.md) (Skill `51088`), donde se resume el valor a facturar del certificado y se genera la solicitud oficial.

## Salida de respaldo

Si el flujo es invocado de manera aislada o la variable `contexto` no coincide con los trámites previstos:
- Deriva de forma segura a [0 - Inicio](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/0-inicio.md) (Skill `51065`), reiniciando la atención para evitar que la conversación quede estancada.
