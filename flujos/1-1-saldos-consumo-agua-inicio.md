# 1.1 - Saldos Consumo Agua - Inicio

Inicia la consulta de saldo de consumo de agua potable. Configura los textos personalizados de pregunta y lista de contratos para este trámite y deriva al flujo central de identificación de contratos.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Consentimiento LOPD: Aceptado y vigente.
- Contexto de navegación: `saldos`.

## Paso 1. Configuración de parámetros y mensajes

El flujo inicializa las variables necesarias en memoria:

1. **Marcador de analítica:** `Saldos Agua`.
2. **Dirección del servicio WSDL:**
   - `urlService`: `http://192.168.10.112/WSInteragua/ServiceInteragua.asmx?WSDL`
3. **Textos dinámicos para la consulta:**
   - Menú de selección (`msm_btn_consultaContrato`):
     > ¿Quieres consultar tu saldo de consumo de agua con tu número de contrato o número de cédula?
   - Lista de contratos (`msm_btn_listaContratos`):
     > A continuación te presentamos los contratos y direcciones asociados a tu identificación. 
     > 
     > ¿Sobre cuál quieres consultar tu saldo de consumo de agua?

## Paso 2. Pausa conversacional

Aplica una pausa visual de 3 segundos para asegurar una transición fluida en el chat antes de presentar el menú de selección.

## Salida

Transfiere inmediatamente con las variables preparadas a:

- [_ Consulta Contratos - Inicio](./consulta-contratos-inicio.md) (Skill ID `51072`).
