# 6.0 - Alta Facturación - Inicio

Punto de entrada al proceso de reclamo formal por consumo excesivo o alta facturación de agua potable. Verifica el estado operativo del servicio, inicializa la estructura de meses para el desglose cronológico de las planillas y prepara el contexto para la validación de políticas de datos.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Alta Facturación* desde el Menú de Reclamos o Menú Principal.

## Paso 1. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- **En mantenimiento (`enMantenimiento`):** Transfiere de inmediato a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- **Operativo (`fueraDeMantenimiento`):** Continúa al Paso 2.

## Paso 2. Marcador analítico e inicialización de calendario

Al confirmar disponibilidad operativa:
1. Se registra el marcador analítico: `Dev Inicio Alta Fact`.
2. Se almacena en memoria el catálogo de meses para la presentación legible de las facturas:
   - `MONTH_NAMES`: `["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"]` (vigencia de 86400 segundos / 24 horas).
3. Se configuran las variables de sesión:
   - `contexto`: `alta_facturacion`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto `alta_facturacion`.
- **En caso de fallo técnico:** Transfiere a [_ Mensaje Error Servicio](./mensaje-error-servicio.md) (Skill ID `51045`).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
