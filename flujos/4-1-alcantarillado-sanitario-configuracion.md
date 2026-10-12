# 4.1 - Alcantarillado Sanitario - Configuración

Configura los parámetros técnicos para la atención de problemas en redes de alcantarillado sanitario (aguas servidas), valida que el usuario cuente con un número celular de contacto, envía una imagen explicativa del servicio, registra el inicio de la gestión en la bitácora de base de datos y guarda en segundo plano la política de tratamiento de datos personales.

## Datos que debe traer al entrar

- `contexto`: `alcantarillado_sanitario`.
- `user.phone`: Teléfono móvil del cliente.
- `paramsLODP`: Parámetros de consentimiento de datos personales.
- `requiredQuestion`: Indicador de inserción de política.

## Paso 1. Verificación de contacto telefónico

El sistema evalúa si la sesión cuenta con el número telefónico del cliente:
- **Si falta el número:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para solicitarlo. Si el cliente desiste o no lo ingresa:
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

## Paso 3. Configuración de parámetros y entrega de imagen de servicio

Se inicializan las variables técnicas en memoria:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `30` (Identificador de tipo de reporte para Alcantarillado Sanitario).

El bot envía una imagen orientativa del servicio de alcantarillado sanitario:
- **Imagen URL:** [https://cdn.jelou.ai/10637/image/6c82505e-bfcd-4a90-b398-0102695e210d-2106cc52-4719-4a18-b0f3-d93ce026dd93-1785951504734.jpeg](https://cdn.jelou.ai/10637/image/6c82505e-bfcd-4a90-b398-0102695e210d-2106cc52-4719-4a18-b0f3-d93ce026dd93-1785951504734.jpeg)

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

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) (Skill ID `51103`) con el tipo de consulta configurado en `30`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
