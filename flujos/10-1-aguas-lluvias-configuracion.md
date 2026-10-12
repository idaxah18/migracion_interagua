# 10.1 - Aguas Lluvias - Configuración

Configura los parámetros técnicos para la atención de incidencias en redes de aguas lluvias y sumideros pluviales. Asegura la captura de número celular de contacto, envía una imagen infográfica del sistema de drenaje, registra el inicio de la gestión en la bitácora operativa de base de datos y guarda en segundo plano la aceptación de la política de datos personales.

## Datos que debe traer al entrar

- `contexto`: `aguas_lluvia`.
- `user.phone`: Teléfono móvil del cliente.
- `paramsLODP`: Parámetros de consentimiento de datos personales.
- `requiredQuestion`: Indicador de registro de política.

## Paso 1. Verificación de contacto telefónico

El sistema valida la existencia de un número celular asociado:
- **Si falta el teléfono:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para solicitarlo. Si el usuario no lo ingresa:
  > No recibimos tu contacto. Escríbenos cuando quieras continuar.
  *(El proceso finaliza en este punto).*
- **Si el teléfono está presente:** Continúa al Paso 2.

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

## Paso 3. Configuración de parámetros y entrega de infografía

Se establecen las variables técnicas del servicio:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `null` (Permite al clasificador abrir las opciones específicas de drenaje pluvial).

El bot envía una imagen orientativa del servicio de drenaje pluvial:
- **Imagen URL:** [https://cdn.jelou.ai/10637/image/c9d0ffcd-c24e-4f01-9ea0-65d181b27c6f-b8e1ec38-7301-4907-956c-80e3fd2cfff5-1785788677215.png](https://cdn.jelou.ai/10637/image/c9d0ffcd-c24e-4f01-9ea0-65d181b27c6f-b8e1ec38-7301-4907-956c-80e3fd2cfff5-1785788677215.png)

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
     "tipo_reclamo": "Aguas Lluvias"
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

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) (Skill ID `51103`) para seleccionar si se trata de limpieza de sumidero o faltante de rejilla.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
