# 12.1 - Venta Servicios - Servicios

Confirma con el cliente los datos del contrato y la dirección de atención, consulta el catálogo de servicios de ingeniería comercial habilitados, presenta las condiciones técnicas y económicas (por ejemplo, tarifas de limpieza y viabilidad de pozos sépticos), y solicita la autenticación del usuario mediante OTP antes de proceder.

## Datos que debe traer al entrar

- `Contrato.ID_CONTRATO`: Código de contrato seleccionado.
- `Contrato.DIRECCION_CORTA`: Dirección abreviada del predio.
- `identificacion`: Cédula o RUC del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `contexto`: `venta_servicios`.

## Paso 1. Confirmación de contrato y dirección

El bot solicita la conformidad del cliente:

> Por favor confírmanos si está correcto el número de contrato y dirección:  
>   
> *Contrato No. {{$memory.Contrato.ID_CONTRATO}} - {{$memory.Contrato.DIRECCION_CORTA}}*  
>   
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Continúa a consultar los datos del cliente y servicios (Paso 2).
2. **No:** Transfiere de regreso a selección de contratos.

## Paso 2. Consulta de datos del cliente y catálogo de servicios

1. **Datos del cliente:**  
   Servicio: [getDatosCliente](../servicios/getDatosCliente.md).  
   Recupera `NOMBRE_BENEFICIARIO`, `DIRECCIONTRADICIONAL` e `IDENTIFICACION_BENEF`.

2. **Catálogo de servicios de ingeniería:**  
   Servicio: [getListadoTipoServicio](../servicios/getListadoTipoServicio.md).

Si no hay servicios disponibles:
> No existen servicios disponibles por el momento  
*(Transfiere a atención adicional).*

Si existen servicios:
- Se genera la lista numerada (`menu_servicios`) con los servicios disponibles (ej. Limpieza de pozo séptico, Mantenimiento de trampa de grasa).
- El usuario ingresa el número de la opción deseada (o `0` para salir).

## Paso 3. Presentación de condiciones técnicas y tarifarias

Según el servicio seleccionado (`op_venta_servicio`):

### Opción Pozo Séptico (`is_pozo_septico === true`):
El bot informa los costos y advertencias técnicas:

> El servicio de limpieza de pozo séptico tiene un valor de $112,23.  
>   
> Antes de solicitar la limpieza, puedes optar por una evaluación de viabilidad por un valor adicional de $14.55. Esta alternativa permite determinar previamente si el servicio puede realizarse.  
>   
> En caso de solicitar directamente la limpieza y que el servicio no se puede ejecutar, se generarán cargos adicionales por movilización.

Y detalla los requisitos físicos obligatorios:
> Antes de continuar, verifica que tu pozo séptico cumpla con las siguientes condiciones:  
> • La abertura de acceso al pozo debe ser de al menos 40 cm x 40 cm.  
> • La tapa de acceso debe encontrarse abierta y sin obstrucciones ni daños estructurales.  
> • Debe existir una distancia máxima de 30 metros entre el pozo y el punto hasta donde pueda llegar el vehículo.  
> • El acceso debe ser adecuado para el ingreso del vehículo, considerando pendientes, callejones, condiciones del acceso vehicular y del terreno.  
> Importante: El servicio contempla únicamente la extracción de líquidos. No incluye la extracción de grasas ni aceites.

### Opción Trampa de Grasa (`is_trampa_grasa === true`):
Presenta la explicación breve del servicio configurada en `EXPLICACION_BREVE`.

## Paso 4. Autenticación de seguridad (OTP)

Para garantizar la legitimidad de la solicitud de servicios con costo facturable:
- El bot transfiere a [_ OTP - Inicio](./otp-inicio.md) para emitir y validar el código temporal de verificación de identidad.

## Salida

- **Continuación normal:** Transfiere a [_ OTP - Inicio](./otp-inicio.md).
- **Si el cliente desiste (Ingresa 0):** Transfiere a [_ Atención Adicional](./atencion-adicional.md).
- **Fallas de servicio:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
