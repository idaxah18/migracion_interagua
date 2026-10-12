# 7.2 - Afectación Servicio - Interrupciones

Confirma los datos del inmueble seleccionado, consulta en el Web Service de Interagua si existen cortes de agua activos, mantenimientos emergentes o interrupciones programadas en el sector del contrato, e informa al cliente los detalles del sector afectado y el tiempo estimado de restablecimiento.

## Datos que debe traer al entrar

- `numeroContrato`: Código de contrato seleccionado.
- `Contrato.DIRECCION_COMPLETA`: Dirección registrada del inmueble.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `contexto`: `afectacion_servicio`.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- En mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Operativo: Continúa al Paso 2.

## Paso 2. Confirmación de contrato y dirección

El bot solicita la conformidad del cliente antes de consultar el sistema:

> Previo al ingreso de tu solicitud, por favor confírmanos si está correcto el número de contrato y dirección:  
>   
> *Contrato No. {{$memory.numeroContrato}} - {{$memory.Contrato.DIRECCION_COMPLETA}}*  
>   
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Continúa a consultar el estado del servicio (Paso 3).
2. **No:** Transfiere de regreso a [_ 7.1 - Afectación Servicio - Contratos](./7-1-afectacion-servicio-contratos.md) (Skill ID `51106`) para seleccionar otro contrato.

## Paso 3. Consulta de interrupción del suministro

Servicio: **Get Info Interrupción** (Tool ID `3025`)  
Endpoint: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "url": "{{$memory.urlService}}",
  "identificacion": "<identificacion>",
  "numeroContrato": "<numeroContrato>"
}
```

Reglas de negocio:
1. **Si existe interrupción en el sector (`has_interrupcion === true`):**
   - Extrae `SECTOR_AFECTADO` y `TIEMPO_AFECTACION`.
   - Muestra el mensaje informativo:
     > Ciudadela: {{$memory.data.SECTOR_AFECTADO}}  
     >   
     > {{$memory.data.TIEMPO_AFECTACION}}
   - Registra el marcador analítico `F Afectacion Servic`.
   - Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
2. **Si NO se registra afectación en el sector:**
   - Muestra el mensaje:
     > No se registra afectación de servicio en tu sector.  
     >   
     > Para mayor información, comunícate con nosotros llamando al 134
   - Seguido de la pregunta con botones:
     > ¿Presentas interrupciones en el servicio?
     - **Contactar agente:** Transfiere a [_ Ayuda Personalizada](./ayuda-personalizada.md) (Skill ID `51108`) para que un operador atienda el reporte.
     - **Salir / No:** Transfiere a [_ Adiós](./adios.md) (Skill ID `51139`) o [_ 0.3 - Menú Principal](./0-3-menu-principal.md).
3. **Falla de conexión o servicio:**  
   Registra el marcador `ES Afectacion Servic` y transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).

## Salida

- **Afectación informada con éxito:** [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **Sin afectación reportada (asistencia manual):** [_ Ayuda Personalizada](./ayuda-personalizada.md) (Skill ID `51108`).
- **Rechazo de datos de contrato:** [_ 7.1 - Afectación Servicio - Contratos](./7-1-afectacion-servicio-contratos.md) (Skill ID `51106`).
- **Fallas técnicas:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
