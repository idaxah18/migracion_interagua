# 5.1 - Fugas de Agua - Configuración

Configura los parámetros técnicos para el reporte de averías de agua potable, asegura que el cliente disponga de teléfono de contacto, entrega una infografía explicativa sobre los tipos de fugas, registra el inicio de la gestión en la base de datos operativa y guarda en segundo plano la aceptación de la política de datos personales.

## Datos que debe traer al entrar

- `contexto`: `fuga_agua`.
- `user.phone`: Teléfono móvil del cliente.
- `paramsLODP`: Parámetros de consentimiento de datos personales.
- `requiredQuestion`: Indicador de registro de política.

## Paso 1. Verificación de contacto telefónico

El sistema valida la existencia de un número celular asociado:
- **Si no tiene teléfono registrado:** Transfiere a [_ Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para solicitarlo. Si el cliente no lo envía:
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

Se establecen las variables técnicas del servicio de reclamos técnicos:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `10` (Identificador de tipo de reporte para averías de Agua Potable).

El bot envía una imagen infográfica descriptiva para orientar la identificación del daño:
- **Imagen URL:** [https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10670%2Fimage%2F4c560008-f363-4027-a30a-3438d9c7c951-1754058965194.png](https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10670%2Fimage%2F4c560008-f363-4027-a30a-3438d9c7c951-1754058965194.png)

Se aplica una pausa técnica de **3 segundos** para que el usuario reciba la imagen.

## Paso 4. Registro de auditoría y guardado de política

1. **Fecha y hora de inicio:**  
   Se captura la marca temporal en zona horaria local (`YYYY-MM-DD HH:mm:ss`) en `context.fecha_hora`.

2. **Registro en base de datos Jelou:**  
   Se inserta un registro en la tabla `9958` (`log_reclamo`):
   ```json
   {
     "canal": "WhatsApp",
     "estado": "inicio",
     "opcion": "Agua y Saneamiento",
     "id_cliente": "{{$user.phone}}",
     "fecha_inicio": "{{$context.fecha_hora}}",
     "tipo_reclamo": "Agua Potable"
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

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) (Skill ID `51103`) con el código de consulta configurado en `10`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
