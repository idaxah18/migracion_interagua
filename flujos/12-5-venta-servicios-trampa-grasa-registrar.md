# 12.5 - Venta Servicios - Trampa Grasa Registrar

Formaliza la solicitud de inspección y registro comercial para trampas de grasa. Solicita los horarios de atención del local, ratifica la aceptación de la tarifa, recibe y valida un archivo PDF con la documentación técnica requerida, procesa observaciones del usuario, invoca el servicio de creación en el sistema comercial, adjunta digitalmente el documento probatorio y confirma la radicación exitosa al cliente.

## Datos que debe traer al entrar

- `datos_cliente`: Información del contrato y beneficiario.
- `nombre_propietario`, `identificacion_propietario`: Datos del titular.
- `ruc_propietario`, `nombre_establecimiento`, `numero_establecimiento`: Datos del local.
- `departamento_selected.VALOR`: Costo estimado del servicio.
- `user.phone`: Teléfono móvil del cliente.

## Paso 1. Verificación de contacto telefónico

El sistema revisa la presencia del teléfono en la sesión:
- **Sin teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para capturarlo. Si el usuario no lo proporciona:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El trámite se interrumpe).*
- **Con teléfono:** Continúa al Paso 2.

## Paso 2. Captura de horarios de atención del local

El bot solicita la jornada laboral del establecimiento:

> Ingresa los días y horarios de funcionamiento de tu local  
> Ej: Lunes a Viernes de 8:00 Am a 17:00 Pm

La respuesta se almacena en la variable `horario_establecimiento`.

## Paso 3. Ratificación del costo del servicio

El bot solicita la conformidad final con el valor presupuestado:

> Este servicio tiene un costo estimado de $ {{$memory.departamento_selected.VALOR}}  
> ¿Estás de acuerdo?

Opciones con botones:
1. **Sí:** Procede a la carga de documentación (Paso 4).
2. **No:** Desiste del requerimiento y finaliza cordialmente.

## Paso 4. Carga de archivo PDF con documentación técnica

El bot solicita al cliente adjuntar el expediente digital del local:
- **Requisito técnico:** Debe subirse estrictamente un único archivo en formato **PDF**.
- Si el usuario envía fotos sueltas, audios o texto:
  > Lo que ingresaste no es correcto, recuerda que debes subir *un solo PDF* con la información solicitada.
- El archivo recibido se almacena temporalmente en `documentacion`.

## Paso 5. Captura opcional de observaciones

El bot pregunta con botones:

> ¡Gracias! ¿Te gustaría agregar alguna observación para tu solicitud?

Opciones con botones:
1. **Sí:** Solicita el texto libre y valida mediante expresión regular que no incluya URLs ni caracteres extraños. Formatea: `comentario = "WH " + telefonoUsuario + " - " + observacion.slice(0, 150)`.
2. **No:** Asigna `comentario = ""` y avanza de inmediato al registro.

## Paso 6. Registro de solicitud y carga de documentos adjuntos

1. **Emisión de la solicitud:**  
   Servicio: **Get Solicitud Trampa Grasas** (Tool ID `6446`)  
   Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
   Método: `POST (SOAP)`  
   Recupera el identificador oficial de expediente: `response_registro.data.INFO_PACKAGE.PACKAGE_ID`.

2. **Asociación del documento PDF:**  
   Servicio: **Get Documentos Imagenes** (Tool ID `6450`)  
   Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
   Método: `POST (SOAP)`  
   Request:
   ```json
   {
     "env": { "value": "Produccion" },
     "codigo": "<PACKAGE_ID>",
     "comentario": "<identificacion>",
     "rutaArchivo": "<documentacion>",
     "nombreArchivo": "<telefono_contacto>.html",
     "identificacion": "<identificacion>"
   }
   ```

## Paso 7. Entrega de confirmación oficial

El bot envía el comprobante de registro:

> Tu solicitud {{$memory.response_registro.data.INFO_PACKAGE.PACKAGE_ID}} ha sido generada con éxito. En las próximas horas nos contactaremos contigo para coordinar la inspección.

## Salida

- **Trámite completado exitosamente:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Fallas en el registro:** Transfiere a [_ Mensaje Error Personalizado](./mensaje-error-personalizado.md) (Skill ID `51091`) o [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
