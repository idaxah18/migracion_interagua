# Validación de código de seguridad (OTP) - Derivación

Se ejecuta cuando el código OTP ya fue validado. No muestra mensajes. Según el trámite que pidió la verificación, devuelve al cliente al registro correspondiente.

## Datos de entrada

- `contexto`: `venta_servicios` o `certificados_administrativos`.
- `is_trampa_grasa`: `true` si el servicio es trampa de grasa.
- `numeroContrato`, `identificacion` y `correo_selected` confirmados en [OTP - Inicio](./otp-inicio.md).

## Enrutamiento

1. **Venta de servicios** (`contexto` = `venta_servicios`):
   - `is_trampa_grasa` en `true`: [12.4 - Venta Servicios - Trampa Grasa](./12-4-venta-servicios-trampa-grasa.md).
   - En cualquier otro caso: [12.3 - Venta Servicios - Registrar](./12-3-venta-servicios-registrar.md).
2. **Certificados administrativos** (`contexto` = `certificados_administrativos`): [13.2 - Certificados - Confirmación](./13-2-certificados-confirmacion.md).

Si el contexto no coincide con esos trámites, se transfiere a [0 - Inicio](./0-inicio.md).
