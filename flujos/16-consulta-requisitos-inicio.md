# 16 - Consulta Requisitos - Inicio

Punto de entrada a la guía informativa de trámites y requisitos legales de Interagua. Prepara el entorno para la consulta de documentación reglamentaria y canaliza hacia la validación de la política de datos personales.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Origen de navegación: Selección de *Consulta de Requisitos* desde el Menú Principal o palabras clave vinculadas a documentación necesaria para trámites.

## Paso 1. Marcador analítico y configuración de contexto

Al iniciar el flujo:
1. Registra el hito analítico: `_ 16 - Consulta Requisitos - Inicio`.
2. Establece las variables en memoria:
   - `contexto`: `consulta_requisitos`
   - `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`
   - `num_intentos_ne`: `0`

## Salida

- **Continuación normal:** Transfiere a [_ 0.1 - LOPD - Inicio](./0-1-lopd-inicio.md) (Skill ID `51066`) para validar el consentimiento de datos personales bajo el contexto `consulta_requisitos`.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
