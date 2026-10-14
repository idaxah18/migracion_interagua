# 3 - Pago de Planilla - Inicio

Entrega el acceso seguro e inmediato a la pasarela oficial de recaudación en línea (botón de pagos) de Interagua para cancelar planillas pendientes con tarjetas de crédito, débito o transferencias bancarias.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Contexto de navegación: `pago_planilla`.

## Paso 1. Registro del hito en analítica

Al ingresar al flujo:
1. Registra el marcador de analítica: `Pago de Planilla`.

## Paso 2. Entrega del enlace de pago

El bot envía el mensaje con el enlace directo al portal de pagos:

> Ingresa a nuestro sistema de pago de forma segura e inmediata dando clic en el siguiente link:  
> 
> [https://botondepagos.interagua.com.ec/online](https://botondepagos.interagua.com.ec/online)  
> 
> Gracias por consultarme.

## Salida

Una vez entregado el enlace:

- Transfiere a: [_ Atención Adicional](./atencion-adicional.md) para consultar si el usuario desea realizar otra gestión.
- En caso de cancelación o reinicio: Transfiere a [_ 0 - Inicio](./0-inicio.md).
