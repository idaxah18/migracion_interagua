# 14 - Agendamiento Citas

Gestiona el calendario y reserva de citas de inspección técnica domiciliaria para reclamos de alta facturación o contratación de servicios de ingeniería. Consulta los turnos libres en el sistema de cuadrillas de Interagua, agrupa los horarios en franjas matutinas y vespertinas en días laborables, solicita la confirmación del usuario y formaliza la reserva mediante el servicio `Set Citas`.

## Datos que debe traer al entrar

- `idOrden` / `orderId`: Identificador único de la orden técnica.
- `identificacion`: Cédula o RUC del solicitante.
- `comentarioAgendamientoCita`: Observación de la cita.
- `numeroSolicitud` / `packageId`: Número de la solicitud previa.
- Contexto de origen (`alta_facturacion` o `venta_servicios`).

## Paso 1. Consulta y procesamiento de turnos disponibles

Servicio: **Get Available Appointments** (Tool ID `8475`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`

Reglas de procesamiento de la agenda:
1. **Filtro de días hábiles:** Se descartan automáticamente sábados y domingos (`day === 0 || day === 6`).
2. **Franjas horarias:** Se clasifican los turnos en:
   - **Mañana:** Turnos con inicio entre las 08:00 y las 12:00.
   - **Tarde:** Turnos con inicio a partir de las 13:00 hasta las 17:00.
3. **Paginación:** Se seleccionan hasta un máximo de 9 opciones (`MAX_OPCIONES = 9`) para adaptarse a las restricciones de mensajes de lista interactiva de WhatsApp.
4. Cada opción se rotula con la estructura: `Día - Mañana / Tarde` (ej. *Lunes 14/04 - Mañana*).

Si no existen citas en el rango:
> No se encontraron horarios disponibles para la fecha consultada.  
*(Transfiere a soporte técnico).*

## Paso 2. Presentación y elección de la cita

El bot despliega la lista interactiva:

> Estas son las fechas y horarios disponibles para tu inspección.  
> Selecciona una opción para continuar:

Al elegir una franja, el bot solicita la confirmación con botones:

> Has seleccionado: {{$memory.appointmentSelected.TEXTO}}  
>   
> ¿Deseas confirmar tu cita?

Opciones con botones:
1. **Confirmar:** Ejecuta la reserva en el Paso 3.
2. **Cambiar horario:** Regresa a la lista para elegir otro turno.

## Paso 3. Registro formal de la cita en agenda

Servicio: **Set Citas** (Tool ID `8476`)  
Endpoint: `https://chatbot.jelou.ai/v1/api/soap`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Desarrollo" },
  "orderId": "<idOrden>",
  "comment1": "<comentarioAgendamientoCita>",
  "dateStart": "<fechaCita>",
  "hourStart": "<horaInicioCita>",
  "identificacion": "<identificacion>",
  "programingCode": "<programingCode>",
  "programingClass": "<programingClass>"
}
```

El servicio responde con HTTP 200 al registrar o actualizar exitosamente la asignación de la cuadrilla.

## Paso 4. Notificación final según el trámite de origen

### Si proviene de Reclamo por Alta Facturación:
> Tu reclamo ha sido grabado con éxito, tu # de solicitud es {{$memory.numeroSolicitud}}.  
>   
> Para seguimiento de tu trámite, puedes consultarlo en el módulo *"Consulta de Solicitudes"* desde el menú principal o escribiendo la palabra *"Solicitudes"*.
- Registra marcador analítico: `FIN Alta Fact`.

### Si proviene de Venta de Servicios de Ingeniería:
> Tu solicitud {{$memory.response_registro.data.PACKAGE_ID}} ha sido generada con éxito. En las próximas horas nos contactaremos contigo para coordinar la inspección.
- Registra marcador analítico: `Venta Servicios - Ex`.

## Salida

- **Cita programada exitosamente:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
