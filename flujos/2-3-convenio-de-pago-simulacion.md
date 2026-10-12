# 2.3 - Convenio de Pago - Simulación

Calcula la liquidación tentativa del convenio de pago (valor exacto de cuota inicial y cuota mensual aproximada) conectando con el servicio de liquidación de Interagua, entrega al cliente el documento PDF oficial de Términos y Condiciones, y solicita su confirmación para proceder con la generación legal del convenio.

## Datos que debe traer al entrar

- `valorDeuda`: Total adeudado por el contrato.
- `porcentaje`: Porcentaje de cuota inicial elegido en el flujo anterior.
- `mesesFinanciar`: Número de cuotas mensuales seleccionadas.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- Si el servicio está en mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Si está operativo: Continúa al Paso 2.

## Paso 2. Cálculo previo de cuota inicial

Antes de consultar el Web Service, el bot efectúa el cálculo aritmético en memoria:

$$\text{cuotaInicial} = \frac{\text{valorDeuda} \times \text{porcentaje}}{100}$$

El resultado se formatea a dos decimales y se asigna a la variable `cuotaInicial`.

## Paso 3. Simulación de cuota con el Web Service

Servicio: **Get Valor Cuota Aproximada** (Tool ID `2916`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "cuotaInicial": "<cuotaInicial>",
  "deudaPendiente": "<valorDeuda>",
  "identificacion": "<identificacion>",
  "mesesFinanciar": "<mesesFinanciar>"
}
```

Validación de la respuesta:
1. **Error técnico o falla de red:**  
   Mensaje: `!Lo sentimos¡, el servicio no está disponible actualmente. Por favor, inténtalo nuevamente más tarde.`
2. **Sin datos devueltos:** Si la respuesta no contiene registros:  
   Mensaje: `No se encontró valor de cuota para el plazo y deuda indicado`
3. Si ocurre algún error: Se registra el marcador `ES Convenio Pago` y se transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).

Si la simulación es exitosa:
- Se capturan las variables devueltas:
  - `cuotaInicial`: Valor de la cuota inicial (`CUOTA_INICIAL`).
  - `cuotaAproximada`: Valor de la cuota mensual proyectada (`CUOTA_APROXIMADA`).
- Se guarda el objeto `dataCuotaAproximada` en memoria durante 3600 segundos.

## Paso 4. Entrega de Términos y Condiciones (PDF)

El bot envía de forma transparente el documento legal en formato PDF:

- **Nombre del documento:** Términos y Condiciones de Convenio de Pago
- **Archivo URL:** [https://s3-us-west-2.amazonaws.com/cdn.devlabs.tech/interagua-gea-documents/Terminos_y_condiciones_de_convenios_de_pagos](https://s3-us-west-2.amazonaws.com/cdn.devlabs.tech/interagua-gea-documents/Terminos_y_condiciones_de_convenios_de_pagos)

Se aplica una breve pausa técnica de **3 segundos** para garantizar que el archivo se descargue y visualice correctamente en el WhatsApp del cliente.

## Paso 5. Confirmación del acuerdo y toma de decisión

El bot presenta el desglose financiero con botones de respuesta:

> ¡Perfecto! Te presento las cuotas y los términos y condiciones del convenio simulado:  
> ▪ Cuota inicial: ${{$memory.cuotaInicial}}  
> ▪ Cuota mensual aproximada: ${{$memory.cuotaAproximada}}  
>   
> ¿Estás de acuerdo?

Opciones con botones:
1. **Sí:** Acepta los valores e inicia la emisión legal. Transfiere a [_ 2.4 - Convenio de Pago - Genera Convenio](./2-4-convenio-de-pago-genera-convenio.md) (Skill ID `51096`).
2. **No:** Desiste del convenio actual y presenta el menú de opciones alternativas:
   > De acuerdo, no se ha generado el convenio de pago.  
   >   
   > ¿Te puedo ayudar en algo más?
   - **Volver a simular:** Transfiere a [_ 2.2 - Convenio de Pago - Porcentaje y Plazo](./2-2-convenio-de-pago-porcentaje-y-plazo.md) (Skill ID `51094`) para cambiar el porcentaje o el plazo.
   - **Contactar agente:** Transfiere a [_ Ayuda Personalizada](./ayuda-personalizada.md) (Skill ID `51108`) para recibir asistencia con un operador.
   - **No, gracias:** Transfiere a [_ Adiós](./adios.md) (Skill ID `51139`) para finalizar la sesión cordialmente.

## Salida

- **Generación aprobada:** [_ 2.4 - Convenio de Pago - Genera Convenio](./2-4-convenio-de-pago-genera-convenio.md) (Skill ID `51096`).
- **Reconfiguración:** [_ 2.2 - Convenio de Pago - Porcentaje y Plazo](./2-2-convenio-de-pago-porcentaje-y-plazo.md) (Skill ID `51094`).
- **Desistimiento o soporte:** [_ Ayuda Personalizada](./ayuda-personalizada.md) o [_ Adiós](./adios.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
