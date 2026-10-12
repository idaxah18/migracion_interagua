# 0.1 - LOPD - Inicio

Verifica si el usuario cuenta con consentimiento vigente para el tratamiento de datos personales conforme a la Ley Orgánica de Protección de Datos Personales (LOPD). Si no lo tiene o ha vencido, presenta el aviso legal y prepara el registro de aceptación antes de continuar.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Nombre de usuario: `{{$user.names}}`.
- Contexto de navegación: `{{$memory.contexto}}` (por defecto `menu_principal` o el trámite que lo originó).
- Canal: WhatsApp / Web.

## Paso 1. Validación de número telefónico

Evalúa si el perfil del usuario cuenta con un número de teléfono válido registrado (`{{$user.phone}}`).

1. **Si no tiene teléfono registrado:**
   - Transfiere al flujo: [Guardia Teléfono](./guardia-telefono.md) (Skill ID `51038`) para solicitar que comparta su contacto.
   - Si no comparte su contacto o el proceso no puede continuar, se muestra el mensaje:
     > No recibimos tu contacto. Escríbenos cuando quieras continuar.
   - El flujo se detiene.

2. **Si tiene teléfono registrado:**
   - Continúa al Paso 2.

## Paso 2. Consulta de política de datos

Servicio: **queryPoliticaDatos** (Tool ID `1567`)

Headers:

apikey = Bearer Token Jelou  
Content-type = application/json  

Endpoint:

`GET https://api.jelou.ai/v2/databases/1611/rows`

Parámetros de consulta (Query params):

```json
{
  "canal": "{{$memory.canal}}",
  "userId": "{{$user.phone}}",
  "searchBy": "userId"
}
```

Reglas de validación de la respuesta:

1. El servicio consulta la tabla de base de datos **1611** (Registros Protección Datos).
2. Si no existen registros previos para este `userId`:
   - `requiredQuestion` = `true` (debe solicitar autorización).
3. Si existe al menos un registro previo:
   - Se toma la fecha del último registro (`fecha`).
   - Se compara con la fecha y hora actual en la zona horaria `America/Guayaquil`.
   - Si ha transcurrido más de 1 año (`now.isAfter(lastFechaLODP + 1 año)`): `requiredQuestion` = `true` (la autorización ha caducado y debe renovarse).
   - Si está dentro del año de vigencia: `requiredQuestion` = `false` (la autorización sigue vigente).

## Paso 3. Presentación de aviso legal (si aplica)

1. **Si `requiredQuestion` es `false` (autorización vigente):**
   - Se guarda en memoria `alreadyAskedTyC` = `true`.
   - Se guarda `requiredQuestion` = `false`.
   - Salta directamente a la derivación de contexto.

2. **Si `requiredQuestion` es `true` (requiere autorización):**
   - El bot envía el aviso legal obligatorio:
     > Al continuar conectado usted autoriza el uso de sus datos personales a Interagua y sus empresas relacionadas. 
     > 
     > Usted puede consultar la política de privacidad en el siguiente link: bit.ly/PPInteragua
   - Se obtiene la fecha actual (`YYYY-MM-DD`) y la hora actual (`HH:mm:ss`) de Ecuador.
   - Se arma el paquete de registro `paramsLODP`:
     ```json
     {
       "canal": "{{$memory.canal}}",
       "fecha": "<fecha actual>",
       "hora": "<hora actual>",
       "userId": "<telefono usuario>",
       "userName": "<nombre usuario>",
       "identificacion": "<identificacion si existe>"
     }
     ```
   - Se guarda `alreadyAskedTyC` = `true` en memoria durante 86400 segundos (24 horas).

## Salida

Una vez procesada la política de datos (sea porque estaba vigente o porque se mostró el aviso legal), el flujo transfiere inmediatamente la conversación a:

- [0.2 - LOPD - Deriva Contexto](./0-2-lopd-deriva-contexto.md) (Skill ID `51067`).
