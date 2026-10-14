# Reclamo Técnico - Contratos

Inyecta los textos interactivos adaptados para la búsqueda y selección de contratos en trámites de reclamos técnicos y averías operativas, transfiriendo la interacción al flujo maestro de contratos.

## Datos que debe traer al entrar

- `tipoConsulta`: Código del tipo de servicio consultado (`10`, `20`, `30`, etc.).
- `tituloServicio`: Nombre descriptivo del daño seleccionado.
- Teléfono del usuario: `{{$user.phone}}`.

## Paso 2. Configuración de textos dinámicos de consulta

El bot inyecta en memoria las preguntas interactivas para la consulta del contrato o identificación:

- `msm_btn_consultaContrato`:
  > ¿Quieres registrar tu reclamo con tu número de cédula o con número de contrato?
- `msm_btn_listaContratos`:
  > A continuación te presentamos los contratos y direcciones asociados a tu identificación.  
  >   
  > ¿Sobre cuál quieres realizar tu reclamo?

## Salida

- **Continuación normal:** Transfiere a [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) para capturar las credenciales y permitir elegir el predio.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
