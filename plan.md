# Organización de presentación: De texto a acción

## 1. Portada

### Idea central

Abrir con una promesa: entender cómo un modelo que solo genera texto puede terminar participando en sistemas que hacen acciones.

### Qué hay ahora

Título principal y subtítulo.

### Texto corto en slide

```text
De texto a acción

Cómo los LLM terminan haciendo cosas
```

### Visual

Una composición simple: a la izquierda una caja negra o nodo llamado `LLM`, saliendo solo texto. A la derecha, borroso o parcialmente oculto, aparecen iconos de herramientas, APIs, documentos y código, como insinuando el destino sin revelarlo completo.

---

## 2. La pregunta que guía todo

### Idea central

Crear tensión narrativa. El público debe quedarse con la pregunta: si el LLM solo predice texto, ¿de dónde salen las acciones?

### Qué hay ahora

Una paradoja inicial.

### Texto corto en slide

```text
Un LLM solo predice texto.

Entonces...
¿cómo termina haciendo cosas?
```

### Visual

Fondo limpio. En el centro, una frase grande: `predict next token`. Alrededor, preguntas pequeñas flotando: `search?`, `tools?`, `APIs?`, `memory?`, `code?`.

---

## 3. El origen: next-token prediction

### Idea central

El punto de partida técnico es simple: el modelo recibe tokens y predice el siguiente token probable.

### Qué hay ahora

Modelo base como predictor de continuidad textual.

### Texto corto en slide

```text
Input:
"The capital of France is"

Next token:
"Paris"
```

### Visual

Una línea horizontal tipo flujo:

```text
tokens in → LLM → next token
```

Debajo, una mini distribución de probabilidad:

```text
Paris  82%
London 4%
Berlin 3%
...
```

Esto puede aparecer animado: primero el input, luego la caja LLM, luego la lista de probabilidades.

---

## 4. Cómo se ve la salida cruda del modelo

### Idea central

La salida del modelo no es una acción ni una decisión externa. Es una continuación de texto.

### Qué hay ahora

Mostrar el modelo generando una secuencia, token por token.

### Texto corto en slide

```text
Prompt:
Explain RAG.

Output:
RAG is a way to...
```

### Visual

Un panel tipo terminal o editor de texto. El output aparece como escritura progresiva, palabra por palabra. No debe parecer chat todavía; debe parecer una continuación textual plana.

---

## 5. De completador de texto a asistente

### Idea central

Un modelo instruct/chat no deja de predecir tokens, pero fue entrenado o alineado para responder instrucciones de forma conversacional.

### Qué hay ahora

Diferencia entre modelo base y modelo de chat.

### Texto corto en slide

```text
Base model:
continues text

Chat model:
follows instructions
```

### Visual

Dos tarjetas lado a lado.

Izquierda:

```text
Base LLM
"Once upon a..."
→ "time..."
```

Derecha:

```text
Chat LLM
User asks
→ Assistant answers
```

Se puede animar el cambio de `completion` a `conversation`.

---

## 6. La interfaz de chat como primer sistema alrededor del modelo

### Idea central

El chat no es solo el modelo. Es una interfaz que empaqueta mensajes con roles y los envía como contexto.

### Qué hay ahora

Introducir `system`, `user`, `assistant`.

### Texto corto en slide

```json
[
  {"role": "system"},
  {"role": "user"},
  {"role": "assistant"}
]
```

### Visual

Una pantalla de chat a la derecha y, a la izquierda, la estructura técnica equivalente. Al hacer click o avanzar, cada burbuja del chat se transforma en una línea JSON corta.

Ejemplo visual:

```text
Chat UI
↓
message list
↓
LLM input
```

---

## 7. Multi-turn: la ilusión de memoria conversacional

### Idea central

El modelo no recuerda mágicamente. El sistema reconstruye el historial y lo manda otra vez como contexto.

### Qué hay ahora

Explicar historial, roles, ventana de contexto y acumulación de turnos.

### Texto corto en slide

```text
Turn 1
User → Assistant

Turn 2
History + new user message

Turn 3
More history + new message
```

### Visual

Tres capas apiladas, como tarjetas acumulándose.

```text
[system]
[user 1]
[assistant 1]
[user 2]
```

Luego una flecha hacia:

```text
LLM context window
```

La animación ideal: cada nuevo turno se agrega al bloque de contexto.

---

## 8. Primer límite importante

### Idea central

El LLM solo puede responder con lo que está en sus pesos o en el contexto actual.

### Qué hay ahora

Preparar la necesidad de RAG.

### Texto corto en slide

```text
El modelo solo ve:

1. entrenamiento
2. contexto actual
```

### Visual

Caja `LLM` con dos fuentes de información entrando:

```text
training data → LLM
prompt context → LLM
```

Afuera de la caja, mostrar documentos privados o información reciente bloqueada con un candado:

```text
new docs
private data
latest info
```

---

## 9. RAG: traer conocimiento externo al contexto

### Idea central

RAG no hace que el modelo “sepa” más internamente; el sistema recupera información y la inserta en el prompt.

### Qué hay ahora

Retrieval → chunks → prompt → respuesta.

### Texto corto en slide

```text
User question
→ retrieve docs
→ add context
→ generate answer
```

### Visual

Un flujo dinámico:

```text
Question
  ↓
Retriever
  ↓
Relevant chunks
  ↓
Prompt + context
  ↓
LLM answer
```

Los chunks pueden verse como pequeñas tarjetas de documentos entrando al contexto.

---

## 10. Cómo cambia la salida con RAG

### Idea central

Comparar la respuesta sin contexto externo vs respuesta aterrizada en documentos recuperados.

### Qué hay ahora

Salida sin RAG y salida con RAG.

### Texto corto en slide

```text
Without RAG:
"I think..."

With RAG:
"According to the document..."
```

### Visual

Split screen.

Izquierda:

```text
LLM only
→ generic answer
```

Derecha:

```text
Retriever + LLM
→ grounded answer
```

Usar una pequeña etiqueta tipo `context included` en el lado RAG.

---

## 11. Segundo límite importante

### Idea central

Aunque RAG trae información, el modelo todavía normalmente produce texto. Puede recomendar una acción, pero no necesariamente ejecutarla.

### Qué hay ahora

Puente hacia salidas estructuradas y herramientas.

### Texto corto en slide

```text
Ahora sabe más.

Pero todavía responde con texto.
```

### Visual

Un LLM diciendo:

```text
"You should search the database."
```

Pero al lado hay una base de datos intacta, sin recibir ninguna llamada. La idea visual es: el modelo “dice”, pero el sistema todavía no “hace”.

---

## 12. Salidas estructuradas y tool calling

### Idea central

La salida del modelo empieza a ser texto diseñado para máquinas. En vez de solo responder al usuario, puede emitir una solicitud estructurada que el sistema puede interpretar.

### Qué hay ahora

Fusiona structured outputs, la pregunta de quién lee el JSON, y tool calling.

### Texto corto en slide

```json
{
  "tool": "search_docs",
  "query": "refund policy"
}
```

Texto complementario mínimo:

```text
The model does not execute.
It requests.
```

### Visual

Tres pasos en una línea:

```text
LLM output
→ parser
→ tool request
```

Mostrar primero una respuesta natural:

```text
"Search the refund policy."
```

Luego transformarla en JSON:

```json
{"tool":"search_docs","query":"refund policy"}
```

Finalmente, el JSON entra en una puerta llamada `harness`.

---

## 13. La pieza invisible: el sistema alrededor del modelo

### Idea central

Alguien tiene que leer, validar, ejecutar y devolver resultados al modelo. Esa capa es la clave, pero todavía se puede presentar como “sistema” antes de nombrarla formalmente.

### Qué hay ahora

La necesidad de una capa externa.

### Texto corto en slide

```text
Read output
Validate
Execute
Return result
```

### Visual

Caja central llamada inicialmente:

```text
System layer
```

Dentro de ella, cuatro mini módulos:

```text
parser
validator
executor
context updater
```

A la izquierda está el `LLM`. A la derecha están las `tools`.

---

## 14. El ciclo de acción: modelo → herramienta → observación

### Idea central

Aquí se resume tool loop + tareas de múltiples pasos + agent loop. El comportamiento agentico aparece cuando el sistema permite repetir el ciclo hasta llegar a una respuesta o criterio de parada.

### Qué hay ahora

Fusiona el ciclo básico, “cuando una acción no basta” y agent loop.

### Texto corto en slide

```text
observe
→ decide
→ act
→ observe
→ stop
```

Ejemplo mínimo:

```text
search → read → compare → answer
```

### Visual

Un loop circular, no una línea recta.

```text
LLM
 ↓ tool call
Tool
 ↓ result
Observation
 ↺ back to LLM
```

En una animación dinámica, primero se muestra una sola vuelta. Luego aparecen varias vueltas pequeñas:

```text
Step 1: search
Step 2: read
Step 3: compare
Step 4: answer
```

---

## 15. Qué es un harness agentico

### Idea central

Revelar el concepto formal: un harness agentico es la capa de software que convierte salidas del modelo en un proceso controlado de acciones, herramientas, estado y validación.

### Qué hay ahora

Definición formal.

### Texto corto en slide

```text
Agentic harness:

the system that turns
model output
into controlled action
```

### Visual

Ahora la caja `System layer` de la slide anterior cambia de nombre a:

```text
Agentic Harness
```

Dentro aparecen módulos:

```text
context
tools
state
memory
validation
execution
```

El efecto visual debe sentirse como una “revelación”.

---

## 16. Anatomía técnica del harness

### Idea central

Mostrar los componentes principales del sistema sin saturar.

### Qué hay ahora

Lista de piezas del harness.

### Texto corto en slide

```text
Context builder
Tool registry
Output parser
Executor
State
Memory
Logs
Limits
```

### Visual

Diagrama tipo arquitectura modular.

```text
User
 ↓
Conversation manager
 ↓
Context builder
 ↓
LLM
 ↓
Output parser
 ↓
Tool router
 ↓
Executor
 ↓
Observation / State
 ↺
```

Para que no sea denso, cada módulo puede ser una tarjeta pequeña con icono. En modo slide deck dinámico, puedes ir iluminando una tarjeta por vez.

---

## 17. Problemas reales y buenas prácticas

### Idea central

Aterrizar la presentación: construir agentes no es solo conectar tools. Hay que controlar errores, permisos, loops, costos y trazabilidad.

### Qué hay ahora

Fusiona problemas reales y buenas prácticas.

### Texto corto en slide

```text
Problems:
loops, bad args, hallucinated tools

Practices:
schemas, limits, logs, validation
```

### Visual

Dos columnas.

Izquierda, en rojo o alerta suave:

```text
Failure modes
- invalid args
- infinite loop
- wrong tool
- long context
```

Derecha, en verde o neutral:

```text
Controls
- strict schema
- step limit
- validation
- traces
```

Puede ser animado como “problema → control”.

---

## 18. Mapa completo del camino

### Idea central

Ahora sí se revela el mapa completo. Funciona como recap final, no como spoiler inicial.

### Qué hay ahora

El recorrido completo desde next-token prediction hasta harness agentico.

### Texto corto en slide

```text
tokens
→ chat
→ context
→ RAG
→ structured output
→ tools
→ loop
→ harness
```

### Visual

Timeline horizontal o camino progresivo. Cada bloque aparece con un pequeño ícono y una mini etiqueta.

Versión compacta:

```text
Next token
  ↓
Chat messages
  ↓
Context
  ↓
RAG
  ↓
JSON / tool call
  ↓
Action loop
  ↓
Agentic harness
```

Esta slide puede reutilizar visualmente elementos ya mostrados antes, como si todo encajara al final.

---

## 19. Cierre

### Idea central

Cerrar con la idea fuerte: el salto de responder a hacer no ocurre solo dentro del modelo, sino en el sistema que interpreta y ejecuta su salida.

### Qué hay ahora

Resumen conceptual final.

### Texto corto en slide

```text
The LLM predicts text.

The harness turns text
into action.
```

### Visual

Una transición simple:

```text
text
→ structure
→ tool call
→ action
```

O una frase final grande sobre fondo oscuro:

```text
Intelligence is not only in the model.
It is also in the system around it.
```

