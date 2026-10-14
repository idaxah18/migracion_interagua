# 10.1 - Aguas Lluvias - Configuración

Configura los parámetros técnicos para la atención de incidencias en redes de aguas lluvias y sumideros pluviales. Asegura la captura de número celular de contacto, envía una imagen infográfica del sistema de drenaje, registra el inicio de la gestión en la bitácora operativa de base de datos.

## Datos que debe traer al entrar

- `contexto`: `aguas_lluvia`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 3. Configuración de parámetros y entrega de infografía

Se establecen las variables técnicas del servicio:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `null` (Permite al clasificador abrir las opciones específicas de drenaje pluvial).

El bot envía una imagen orientativa del servicio de drenaje pluvial:
- **Imagen URL:** [https://cdn.jelou.ai/10637/image/c9d0ffcd-c24e-4f01-9ea0-65d181b27c6f-b8e1ec38-7301-4907-956c-80e3fd2cfff5-1785788677215.png](https://cdn.jelou.ai/10637/image/c9d0ffcd-c24e-4f01-9ea0-65d181b27c6f-b8e1ec38-7301-4907-956c-80e3fd2cfff5-1785788677215.png)

## Paso 4. Registro de auditoría

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

## Salida

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) para seleccionar si se trata de limpieza de sumidero o faltante de rejilla.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
