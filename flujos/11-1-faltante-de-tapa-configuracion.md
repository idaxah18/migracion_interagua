# 11.1 - Faltante de Tapa - Configuración

Configura los parámetros específicos de tipificación para reclamos por sustracción o daño de tapas de alcantarillado (`Causal 3137`, `Paquete 59`), valida que el usuario posea un número celular registrado, genera el registro de inicio en la base de datos operativa y transfiere directamente al flujo de consulta de contratos.

## Datos que debe traer al entrar

- `contexto`: `faltante_tapa`.
- `user.phone`: Teléfono móvil del cliente.
- `paramsLODP`: Parámetros de consentimiento de datos personales.
- `requiredQuestion`: Indicador de inserción de política.

## Paso 1. Verificación de contacto telefónico

El sistema revisa la disponibilidad del número telefónico del cliente:
- **Si falta el número:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para capturarlo. Si el usuario no lo proporciona:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El proceso finaliza en este punto).*
- **Si el número está presente:** Continúa al Paso 2.

## Paso 2. Verificación de horario de mantenimiento

Servicio: **Horario de Mantenimiento** (Tool ID `6070`)  
Request:
```json
{
  "fechaInicio": "11/03/2025 15:00:00",
  "fechaFin": "11/04/2025 10:00:00"
}
```

- En mantenimiento: Transfiere a [_ Mensaje Mantenimiento](./mensaje-mantenimiento.md) (Skill ID `51131`).
- Operativo: Continúa al Paso 3.

## Paso 3. Configuración de parámetros específicos de causal

Se configuran en memoria los códigos técnicos predeterminados de la avería:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `codigoCausal`: `3137`
- `tipoConsulta`: `null`
- `descTipoCausal`: `544 - FALTA DE TAPA DE BOCAS DE REGISTRO CAJA DOMICILIARIA`
- `tituloServicio`: `Faltante de tapa de alcantarilla`
- `codigoTipoPaquete`: `59`

## Paso 4. Registro de auditoría y guardado de política

1. **Fecha y hora de inicio:**  
   Se genera la marca temporal local (`YYYY-MM-DD HH:mm:ss`) en `context.fecha_hora`.

2. **Registro en base de datos Jelou:**  
   Se inserta un registro en la tabla `9958` (`log_reclamo`):
   ```json
   {
     "canal": "WhatsApp",
     "estado": "inicio",
     "opcion": "Agua y Saneamiento",
     "id_cliente": "{{$user.phone}}",
     "fecha_inicio": "{{$context.fecha_hora}}",
     "tipo_reclamo": "Alcantarillado Sanitario"
   }
   ```

3. **Persistencia de política LOPD:**  
   Servicio: **SavePoliticaDatos** (Tool ID `1545`)  
   Endpoint: `https://api.jelou.ai/v2/databases/1611/rows`  
   Método: `POST`  
   Request:
   ```json
   {
     "params": "{{$memory.paramsLODP}}",
     "requiredInsert": "{{$memory.requiredQuestion}}"
   }
   ```

## Salida

- **Continuación normal:** Transfiere directamente a [_ Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md) (Skill ID `51104`), omitiendo los menús genéricos intermedios para proceder a asociar el contrato o predio afectado.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
