# 11.1 - Faltante de Tapa - Configuración

Configura los parámetros específicos de tipificación para reclamos por sustracción o daño de tapas de alcantarillado (`Causal 3137`, `Paquete 59`), valida que el usuario posea un número celular registrado, genera el registro de inicio en la base de datos operativa y transfiere directamente al flujo de consulta de contratos.

## Datos que debe traer al entrar

- `contexto`: `faltante_tapa`.
- `user.phone`: Teléfono móvil del cliente.

## Paso 3. Configuración de parámetros específicos de causal

Se configuran en memoria los códigos técnicos predeterminados de la avería:
- `urlService`: `http://192.168.10.148:8081/WSInteragua/ServiceInteragua.asmx?WSDL`
- `codigoCausal`: `3137`
- `tipoConsulta`: `null`
- `descTipoCausal`: `544 - FALTA DE TAPA DE BOCAS DE REGISTRO CAJA DOMICILIARIA`
- `tituloServicio`: `Faltante de tapa de alcantarilla`
- `codigoTipoPaquete`: `59`

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

- **Continuación normal:** Transfiere directamente a [_ Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md), omitiendo los menús genéricos intermedios para proceder a asociar el contrato o predio afectado.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
