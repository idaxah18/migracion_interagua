# Reclamo Técnico - Inicio

Enrutador dinámico que clasifica las averías operativas en subtipos específicos de daño según la categoría de servicio consultada (Agua Potable, Alcantarillado Sanitario o Drenaje Pluvial). Actualiza el log de reclamos en la base de datos y transfiere al flujo de identificación de contratos.

## Datos que debe traer al entrar

- `tipoConsulta`: Código numérico de categoría (`10` para Agua Potable, `30` para Alcantarillado Sanitario, u otros para Aguas Lluvias).
- `log_reclamo._id`: Identificador del registro creado en la tabla `9958`.
- `user.phone`: Teléfono del usuario.

## Paso 2. Clasificación según la categoría de servicio

El bot evalúa `tipoConsulta` y despliega la lista correspondiente:

### Rama A: Agua Potable (`tipoConsulta === '10'`)
Lista interactiva:
> ¿En qué tipo de reclamo te puedo atender?

Opciones:
- **Fuga Medidor:** Registra marcador `Fuga Medidor`.
- **Fuga Agua Calle:** Registra marcador `Fuga Agua Calle`.
- **Baja Presión:** Registra marcador `Baja Presion`.

### Rama B: Alcantarillado Sanitario (`tipoConsulta === '30'`)
Lista interactiva:
> ¿En qué tipo de reclamo te puedo atender?

Opciones:
- **Desborde:** Configura `tipoConsulta: "20"` y abre submenú:
  > ¿Qué tipo de desborde presentas?
  - *Desborde Acera:* Registra marcador `Desborde Acera`.
  - *Desborde Calle:* Registra marcador `Desborde Calle`.
  - *Desborde Interior:* Registra marcador `Desborde Interior`.
- **Limpieza de Alcantarillado:** Registra marcador `Limpieza Alcant`.
- **Faltante de Tapa:** Registra marcador `Faltante Tapa`.

### Rama C: Aguas Lluvias / Drenaje Pluvial (Default)
Lista interactiva:
> ¿En qué tipo de reclamo te puedo atender?

Opciones:
- **Limpieza de Sumidero:** Registra marcador `Limpieza Sumidero`.
- **Faltante de Rejilla:** Registra marcador `Faltante Rejilla`.

## Paso 3. Actualización de subtipo en bitácora de base de datos

Al seleccionar cualquiera de los daños específicos, el sistema actualiza el registro en la tabla `9958` (`log_reclamo`):

Operación: **Update**  
Tabla ID: `9958`  
Filtro: `id: "{{$memory.log_reclamo._id}}"`  
Datos actualizados:
```json
{
  "subtipo_reclamo": "{{$memory.tituloServicio}}"
}
```

## Salida

- **Continuación normal:** Transfiere a [_ Reclamo Técnico - Contratos](./reclamo-tecnico-contratos.md).
- **Si el usuario cancela:** Transfiere a [_ 0.3 - Menú Principal - Bienvenida](./0-3-menu-principal-bienvenida.md).
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md).
