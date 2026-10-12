# Atención Telefónica

Informa al cliente sobre los canales de atención verbal y la línea telefónica directa oficial de Interagua (Call Center 134) para consultas complejas o emergencias.

## Datos que debe traer al entrar

- Teléfono del usuario: `{{$user.phone}}`.
- Solicitud de atención por voz o derivación desde menús de asistencia.

## Paso 1. Entrega del número de Call Center

El bot envía el mensaje con la línea de contacto:

> Si necesitas hablar con uno de nuestros asesores, puedes comunicarte con nuestro Call Center marcando el 134 desde tu teléfono.  
> 📞 Estamos para ayudarte.

## Salida

- **Continuación normal:** Transfiere a [_ Atención Adicional](./atencion-adicional.md) (Skill ID `51074`) para consultar si el usuario desea realizar alguna gestión adicional por WhatsApp.
- **En caso de reinicio:** Transfiere a [_ 0 - Inicio](./0-inicio.md) (Skill ID `51065`).
