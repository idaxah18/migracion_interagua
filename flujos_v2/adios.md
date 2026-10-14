# Despedida (Adiós)

Este flujo se encarga de despedir cordialmente al cliente cuando este indica que no desea realizar más trámites, cuando selecciona la opción de salida en el menú de atención adicional o cuando escribe una frase de despedida. Para ofrecer una experiencia humana, cercana y dinámica, el bot no repite siempre la misma frase, sino que selecciona de forma aleatoria entre cuatro expresiones de despedida.

## Datos que debe traer al entrar

- No requiere parámetros de entrada obligatorios.
- Se invoca generalmente al responder **"No"** en el flujo de [Atención Adicional](./atencion-adicional.md) o al digitar `0` en menús principales.

## Selección aleatoria del mensaje de despedida

El sistema evalúa un nodo de distribución probabilística uniforme (25% de probabilidad para cada variante) y envía uno de los siguientes cuatro mensajes:

- **Variante 1:**
  > Gracias por consultarme, espero que te haya servido mi ayuda, hasta la próxima! 😊

- **Variante 2:**
  > Gracias por consultarme, recuerda que estoy aquí 24/7 lista para servirte, hasta la próxima! 😊

- **Variante 3:**
  > Gracias por consultarme, que tengas un maravilloso día.

- **Variante 4:**
  > Gracias por consultarme, ¡hasta pronto!.

## Salida y fin de la conversación

- **Cierre del chat:** Tras emitir el mensaje seleccionado, el flujo concluye la sesión activa sin formular más preguntas ni mantener procesos pendientes.
- **Nueva interacción:** Si el cliente vuelve a escribir un mensaje en el futuro, la conversación se iniciará desde cero a través de [0 - Inicio](./0-inicio.md).
