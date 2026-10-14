# 6.1 - Alta Facturación - Contratos

Educa al cliente mediante un video interactivo sobre la prevalencia de fugas internas no detectadas antes de formalizar un reclamo por valores elevados, configura los parámetros del Web Service y transfiere al flujo de contratos con los textos de diálogo adaptados para reclamos de facturación.

## Datos que debe traer al entrar

- `contexto`: `alta_facturacion`.
- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento de tratamiento de datos (LOPD) confirmado.

## Paso 2. Entrega de video de autodiagnóstico de fugas

El bot envía un mensaje multimedia con video y texto introductorio:

- **Archivo de video:** [https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10774%2Fvideo%2F535779f3-90ad-408b-ad81-555f5dfbe5d3-1754058406299.mp4](https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10774%2Fvideo%2F535779f3-90ad-408b-ad81-555f5dfbe5d3-1754058406299.mp4)
- **Texto de acompañamiento (Caption):**
  > Sabías que el 98% de los reclamos por alta facturación corresponden a una fuga interna, te invitamos a ver el siguiente video para un autodiagnóstico.

Se aplica una pausa de **5 segundos** para permitir la carga y reproducción del contenido en la aplicación del cliente.

## Paso 3. Configuración de parámetros y textos dinámicos

Se inicializan las variables técnicas en memoria:
- `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`
- `descTipoCausal`: `503 - ALTA FACTURACIÓN`

Se inyectan los textos de consulta para el selector de contratos:
- `msm_btn_consultaContrato`:
  > ¿Quieres registrar tu reclamo con tu número de cédula o con número de contrato?
- `msm_btn_listaContratos`:
  > A continuación te presentamos los contratos y direcciones asociados a tu identificación.  
  >   
  > ¿Sobre cuál quieres registrar tu reclamo?

## Salida

- **Continuación normal:** Transfiere a [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) para la búsqueda y selección del contrato o cédula.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
