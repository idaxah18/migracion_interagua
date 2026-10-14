# 5.1 - Fugas de Agua - Configuración

Configura los parámetros técnicos para el reporte de averías de agua potable, entrega una infografía explicativa sobre los tipos de fugas, registra el inicio de la gestión en la base de datos operativa.

## Datos que debe traer al entrar

- `contexto`: `fuga_agua`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 3. Configuración de parámetros y entrega de infografía

Se establecen las variables técnicas del servicio de reclamos técnicos:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `10` (Identificador de tipo de reporte para averías de Agua Potable).

El bot envía una imagen infográfica descriptiva para orientar la identificación del daño:
- **Imagen URL:** [https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10670%2Fimage%2F4c560008-f363-4027-a30a-3438d9c7c951-1754058965194.png](https://s3.us-west-2.amazonaws.com/cdn.devlabs.tech/10670%2Fimage%2F4c560008-f363-4027-a30a-3438d9c7c951-1754058965194.png)

Se aplica una pausa técnica de **3 segundos** para que el usuario reciba la imagen.

## Paso 4. Registro de auditoría

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

## Salida

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) con el código de consulta configurado en `10`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
