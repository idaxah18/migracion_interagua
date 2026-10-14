# 4.1 - Alcantarillado Sanitario - Configuración

Configura los parámetros técnicos para la atención de problemas en redes de alcantarillado sanitario (aguas servidas), envía una imagen explicativa del servicio, registra el inicio de la gestión en la bitácora de base de datos.

## Datos que debe traer al entrar

- `contexto`: `alcantarillado_sanitario`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 3. Configuración de parámetros y entrega de imagen de servicio

Se inicializan las variables técnicas en memoria:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `tipoConsulta`: `30` (Identificador de tipo de reporte para Alcantarillado Sanitario).

El bot envía una imagen orientativa del servicio de alcantarillado sanitario:
- **Imagen URL:** [https://cdn.jelou.ai/10637/image/6c82505e-bfcd-4a90-b398-0102695e210d-2106cc52-4719-4a18-b0f3-d93ce026dd93-1785951504734.jpeg](https://cdn.jelou.ai/10637/image/6c82505e-bfcd-4a90-b398-0102695e210d-2106cc52-4719-4a18-b0f3-d93ce026dd93-1785951504734.jpeg)

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
     "tipo_reclamo": "Alcantarillado Sanitario"
   }
   ```

## Salida

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Inicio](./reclamo-tecnico-inicio.md) con el tipo de consulta configurado en `30`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
