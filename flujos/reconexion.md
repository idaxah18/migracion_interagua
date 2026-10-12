# Reconexión del servicio

Este flujo brinda información oficial, clara y precisa al cliente acerca de los tiempos reglamentarios y las condiciones técnicas para el restablecimiento del suministro de agua potable luego de haberse cancelado una deuda pendiente o regularizado un corte administrativo.

## Datos que debe traer al entrar

- No requiere parámetros técnicos obligatorios de entrada.
- Al iniciar, el flujo reinicia en memoria el contador de intentos no entendidos (`num_intentos_ne = 0`) para garantizar una interacción limpia en caso de consultas posteriores.

## Información proporcionada al cliente

El bot entrega el siguiente mensaje explicativo con las directrices operativas de Interagua:

> La reconexión se ejecutará en un plazo máximo de 24 horas laborables después del pago o regularizada la deuda.
> 
> Importante: Cuando la empresa ejecute un cierre definitivo por deuda o por reconexión ilegal del servicio, le corresponde al usuario solicitar una nueva instalación, cuyo tiempo de atención es de 15 días.
> 
> Gracias por consultarme, que tengas un maravilloso día.

### Aspectos clave destacados en el mensaje:
- **Plazo estándar de reconexión:** Máximo 24 horas laborables a partir de la confirmación del pago en los sistemas comerciales.
- **Cierre definitivo o corte por reconexión clandestina:** En estos casos no aplica una simple reconexión; el titular debe solicitar un trámite de nueva instalación domiciliaria, cuyo plazo técnico de ejecución es de hasta 15 días.

## Salida

Tras entregar la información:
- Se deriva de manera automática al flujo de [Atención Adicional](file:///c:/Users/alvarezdx/Documents/AGY/migracion_interagua/flujos/atencion-adicional.md) (Skill `51074`), donde se consulta al usuario si requiere gestionar otro trámite o si da por concluida la conversación.
