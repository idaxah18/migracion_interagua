# 13.1 - Certificados - Precio

Confirma con el cliente los datos del contrato y la dirección del inmueble, expone la tarifa administrativa fijada para la emisión de certificados oficiales ($1.45), y canaliza hacia la verificación de identidad mediante código de un solo uso (OTP).

## Datos que debe traer al entrar

- `Contrato.ID_CONTRATO`: Código de contrato seleccionado.
- `Contrato.DIRECCION_CORTA`: Dirección abreviada del predio.
- `identificacion`: Identificación del solicitante.
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`.
- `contexto`: `certificados_administrativos`.

## Paso 1. Confirmación de contrato y dirección

El bot solicita la conformidad del cliente:

> Por favor confírmanos si está correcto el número de contrato y dirección:  
>   
> *Contrato No. {{$memory.Contrato.ID_CONTRATO}} - {{$memory.Contrato.DIRECCION_CORTA}}*  
>   
> ¿Estás de acuerdo con la información ingresada?

Opciones con botones:
1. **Sí:** Procede a informar la tarifa del certificado (Paso 2).
2. **No:** Transfiere de regreso a [_ 13 - Certificados - Inicio](./13-certificados-inicio.md) (Skill ID `51084`).

## Paso 2. Notificación de tarifa del trámite

El bot presenta el costo reglamentario del documento:

> Este servicio tiene un costo estimado de $1.45  
>   
> ¿Estás de acuerdo?

Opciones con botones:
1. **Sí:** Acepta el cobro del certificado y transfiere a la verificación de identidad [_ OTP - Inicio](./otp-inicio.md) (Skill ID `51079`).
2. **No:** Desiste de la solicitud:
   > Entiendo que no te encuentres de acuerdo. Si cambias de opinión, no dudes en escribirme. 👋  
   *(Finaliza la conversación).*

## Salida

- **Aprobación de tarifa:** Transfiere a [_ OTP - Inicio](./otp-inicio.md) (Skill ID `51079`).
- **Rechazo de contrato:** [_ 13 - Certificados - Inicio](./13-certificados-inicio.md) (Skill ID `51084`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
