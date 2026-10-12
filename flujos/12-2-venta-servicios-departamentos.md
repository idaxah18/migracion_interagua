# 12.2 - Venta Servicios - Departamentos

Consulta y presenta las zonas o departamentos operativos asignados al tipo de inspección técnica, expone la tarifa estimada del servicio solicitado y solicita la aceptación expresa del valor por parte del cliente antes del registro final.

## Datos que debe traer al entrar

- `identificacion`: Cédula o RUC del solicitante.
- `servicio_selected.ID_TIPO_INSPECCION`: Identificador del tipo de inspección.
- `is_trampa_grasa`: Indicador de servicio de trampa de grasa.
- `urlService`: URL del servicio SOAP.

## Paso 1. Consulta de departamentos y tarifas operativas

Servicio: **Get Consulta Departamentos** (Tool ID `6418`)  
Endpoint: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`  
Método: `POST (SOAP)`  
Request:
```json
{
  "env": { "value": "Produccion" },
  "identificacion": "<identificacion>",
  "id_tipo_inspeccion": "<ID_TIPO_INSPECCION>"
}
```

- Si no existen departamentos configurados:  
  > No existen departamentos disponibles por el momento  
  *(Transfiere a atención adicional).*

- Si existe un solo departamento: Se selecciona automáticamente.
- Si existen múltiples departamentos: El bot presenta un menú numerado para elegir el sector de atención y asigna `departamento_selected`.

## Paso 2. Confirmación de costos estimados

El bot presenta la tarifa proyectada:

> Este servicio tiene un costo estimado de $ {{$memory.departamento_selected.VALOR}}.  
> {{$memory.departamento_selected.DESCRIPCION}}  
>   
> ¿Estás de acuerdo?

Opciones con botones:
1. **Sí:** Acepta los valores.
   - Si es Trampa de Grasa (`is_trampa_grasa === true`): Transfiere a [_ 12.5 - Venta Servicios - Trampa Grasa Registrar](./12-5-venta-servicios-trampa-grasa-registrar.md) (Skill ID `51083`).
   - Si es otro servicio (ej. Pozo Séptico): Transfiere a [_ 12.3 - Venta Servicios - Registrar](./12-3-venta-servicios-registrar.md) (Skill ID `51081`).
2. **No:** Desiste de la contratación:
   > Entiendo que no te encuentres de acuerdo. Si cambias de opinión, no dudes en escribirme. 👋  
   *(Transfiere a [_ Adiós](./adios.md)).*

## Salida

- **Aceptación Trampa de Grasa:** [_ 12.5 - Venta Servicios - Trampa Grasa Registrar](./12-5-venta-servicios-trampa-grasa-registrar.md) (Skill ID `51083`).
- **Aceptación otros servicios:** [_ 12.3 - Venta Servicios - Registrar](./12-3-venta-servicios-registrar.md) (Skill ID `51081`).
- **Rechazo / Desistimiento:** Transfiere a [_ Adiós](./adios.md) (Skill ID `51139`) o [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
