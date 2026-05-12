---

name: llm-council

description: "Pasa cualquier pregunta, idea o decisión por un consejo de 5 asesores de IA que la analizan de forma independiente, se revisan entre sí de forma anónima y sintetizan un veredicto final. Basado en la metodología LLM Council de Karpathy. ACTIVADORES OBLIGATORIOS: 'consejo esto', 'ejecuta el consejo', 'guerra de ideas sobre esto', 'ponlo a prueba', 'estresa esto', 'debate esto'. ACTIVADORES FUERTES (usar cuando se combina con una decisión real o una disyuntiva): 'debería hacer X o Y', 'cuál opción', 'qué harías tú', '¿es este el movimiento correcto?', 'valida esto', 'dame múltiples perspectivas', 'no puedo decidir', 'estoy dudando entre'. NO activar en preguntas simples de sí/no, búsquedas de datos, o 'debería' casual sin una disyuntiva real (ej: 'debería usar markdown' no es una pregunta de consejo). SÍ activar cuando el usuario presenta una decisión genuina con consecuencias, múltiples opciones y contexto que sugiere que quiere presión desde múltiples ángulos."

---


# LLM Council


Le haces una pregunta a una IA, obtienes una respuesta. Esa respuesta puede ser genial. Puede ser mediocre. No tienes forma de saberlo porque solo viste una perspectiva.

El consejo soluciona esto. Pasa tu pregunta por 5 asesores independientes, cada uno pensando desde un ángulo fundamentalmente diferente. Luego se revisan entre sí. Luego un presidente sintetiza todo en una recomendación final que te dice dónde están de acuerdo, dónde chocan, y qué deberías hacer realmente.

Esto está adaptado del LLM Council de Andrej Karpathy. Él despacha consultas a múltiples modelos, los hace revisarse entre sí de forma anónima, y luego un presidente produce la respuesta final. Hacemos lo mismo dentro de Claude usando sub-agentes con diferentes lentes de pensamiento en lugar de diferentes modelos.


---


## cuándo ejecutar el consejo


El consejo es para preguntas donde equivocarse es costoso.

Buenas preguntas para el consejo:
- "¿Debería lanzar un workshop de $97 o un curso de $497?"
- "¿Cuál de estos 3 ángulos de posicionamiento es más fuerte?"
- "Estoy pensando en pivotar de X a Y. ¿Estoy loco?"
- "Aquí está el copy de mi landing page. ¿Qué está débil?"
- "¿Debería contratar un VA o primero construir una automatización?"

Malas preguntas para el consejo:
- "¿Cuál es la capital de Francia?" (una sola respuesta correcta, no necesita perspectivas)
- "Escríbeme un tweet" (tarea de creación, no una decisión)
- "Resume este artículo" (tarea de procesamiento, no de juicio)

El consejo brilla cuando hay incertidumbre genuina y el costo de una mala decisión es alto. Si ya sabes la respuesta y solo quieres validación, el consejo probablemente te dirá cosas que no quieres escuchar. Ese es el punto.


---


## los cinco asesores


Cada asesor piensa desde un ángulo diferente. No son títulos de trabajo ni personas. Son estilos de pensamiento que naturalmente crean tensión entre sí.


### 1. El Contrario

Busca activamente qué está mal, qué falta, qué va a fallar. Asume que la idea tiene un defecto fatal e intenta encontrarlo. Si todo parece sólido, cava más profundo. El Contrario no es un pesimista. Es el amigo que te salva de un mal negocio haciendo las preguntas que estás evitando.


### 2. El Pensador de Primeros Principios

Ignora la pregunta superficial y pregunta "¿qué estamos tratando de resolver realmente?" Elimina suposiciones. Reconstruye el problema desde cero. A veces el output más valioso del consejo es que el Pensador de Primeros Principios diga "estás haciendo la pregunta equivocada."


### 3. El Expansionista

Busca el lado positivo que todos los demás están perdiendo. ¿Qué podría ser más grande? ¿Qué oportunidad adyacente está escondida? ¿Qué se está subvalorando? Al Expansionista no le importa el riesgo (ese es el trabajo del Contrario). Le importa qué pasa si esto funciona incluso mejor de lo esperado.


### 4. El Externo

No tiene contexto sobre ti, tu campo ni tu historia. Responde puramente a lo que tiene enfrente. Este es el asesor más subestimado. Los expertos desarrollan puntos ciegos. El Externo detecta la maldición del conocimiento: cosas que son obvias para ti pero confusas para todos los demás.


### 5. El Ejecutor

Solo le importa una cosa: ¿se puede hacer esto realmente, y cuál es el camino más rápido para hacerlo? Ignora la teoría, la estrategia y el pensamiento macro. El Ejecutor analiza cada idea desde el lente de "¿pero qué haces el lunes por la mañana?" Si una idea suena brillante pero no tiene un primer paso claro, el Ejecutor lo dirá.


**Por qué estos cinco:** crean tres tensiones naturales. Contrario vs. Expansionista (desventaja vs. ventaja). Primeros Principios vs. Ejecutor (repensar todo vs. simplemente hazlo). El Externo está en el medio manteniendo a todos honestos al ver lo que ven los ojos frescos.


---


## cómo funciona una sesión de consejo


### paso 1: enmarcar la pregunta (con enriquecimiento de contexto)


Cuando el usuario diga "consejo esto" (o cualquier frase de activación), haz dos cosas antes de enmarcar:


**A. Escanea el espacio de trabajo en busca de contexto.** La pregunta del usuario suele ser solo la punta del iceberg. La configuración de Claude del usuario probablemente contiene archivos que mejorarían dramáticamente el output del consejo. Antes de enmarcar, escanea rápidamente y lee cualquier archivo de contexto relevante:

- `CLAUDE.md` o `claude.md` en la raíz del proyecto o espacio de trabajo (contexto del negocio, preferencias, restricciones)
- Cualquier carpeta `memory/` (perfiles de audiencia, documentos de voz, detalles del negocio, decisiones pasadas)
- Cualquier archivo que el usuario haya referenciado o adjuntado explícitamente
- Transcripciones recientes de consejos en esta carpeta (para no volver a aconsejar el mismo terreno)
- Cualquier otro archivo de contexto relevante para la pregunta específica (ej: si preguntan sobre precios, buscar datos de ingresos, resultados de lanzamientos pasados, investigación de audiencia)

Usa `Glob` y llamadas rápidas de `Read` para encontrarlos. No dediques más de 30 segundos a esto. Buscas los 2-3 archivos que darían a los asesores el contexto necesario para dar consejos específicos y fundamentados en lugar de perspectivas genéricas.


**B. Enmarca la pregunta.** Toma la pregunta cruda del usuario Y el contexto enriquecido y reformúlala como un prompt claro y neutral que recibirán los cinco asesores. La pregunta enmarcada debe incluir:

1. La decisión o pregunta central
2. Contexto clave del mensaje del usuario
3. Contexto clave de los archivos del espacio de trabajo (etapa del negocio, audiencia, restricciones, resultados pasados, números relevantes)
4. Lo que está en juego (por qué esta decisión importa)

No añadas tu propia opinión. No la orientes. Pero SÍ asegúrate de que cada asesor tenga suficiente contexto para dar una respuesta específica y fundamentada en lugar de consejo genérico.

Si la pregunta es demasiado vaga ("consejo esto: mi negocio"), haz una pregunta de aclaración. Solo una. Luego procede.

Guarda la pregunta enmarcada para la transcripción.


### paso 2: convocar el consejo (5 sub-agentes en paralelo)


Genera los 5 asesores simultáneamente como sub-agentes. Cada uno recibe:

1. Su identidad de asesor y estilo de pensamiento (de las descripciones anteriores)
2. La pregunta enmarcada
3. Una instrucción clara: responde de forma independiente. No te vayas por las ramas. No intentes ser equilibrado. Inclínate completamente hacia tu perspectiva asignada. Si ves un defecto fatal, dilo. Si ves una ventaja masiva, dilo. Tu trabajo es representar tu ángulo con la mayor fuerza posible. La síntesis viene después.

Cada asesor debe producir una respuesta de 150-300 palabras. Suficientemente larga para ser sustancial, suficientemente corta para ser escaneable.


**Plantilla de prompt para sub-agentes:**
### paso 3: revisión entre pares (5 sub-agentes en paralelo)


Este es el paso que convierte al consejo en algo más que "preguntar 5 veces." Es el núcleo del insight de Karpathy.

Recoge las 5 respuestas de los asesores. Anonimízalas como Respuesta A hasta E (aleatoriza qué asesor corresponde a qué letra para que no haya sesgo posicional).

Genera 5 nuevos sub-agentes, uno por asesor. Cada revisor ve las 5 respuestas anonimizadas y responde tres preguntas:

1. ¿Cuál respuesta es la más sólida y por qué? (elige una)
2. ¿Cuál respuesta tiene el mayor punto ciego y cuál es?
3. ¿Qué omitieron TODAS las respuestas que el consejo debería considerar?


**Plantilla de prompt para revisores:**
### paso 4: síntesis del presidente


Este es el paso final. Un agente recibe todo: la pregunta original, las 5 respuestas de los asesores (ahora des-anonimizadas para que puedas ver qué asesor dijo qué), y las 5 revisiones entre pares.

El trabajo del presidente es producir el output final del consejo. Sigue esta estructura:


**VEREDICTO DEL CONSEJO**

1. **Dónde está de acuerdo el consejo** — los puntos en que múltiples asesores convergieron de forma independiente. Estas son señales de alta confianza.

2. **Dónde choca el consejo** — los desacuerdos genuinos. No los suavices. Presenta ambos lados y explica por qué asesores razonables están en desacuerdo.

3. **Puntos ciegos que captó el consejo** — cosas que solo emergieron en la ronda de revisión entre pares. Cosas que asesores individuales omitieron y que otros señalaron.

4. **La recomendación** — una recomendación clara y accionable. No "depende." No "considera ambos lados." Una respuesta real. El presidente puede estar en desacuerdo con la mayoría si el razonamiento lo respalda.

5. **Lo primero que debes hacer** — un único próximo paso concreto. No una lista de 10 cosas. Una sola cosa.


**Plantilla de prompt para el presidente:**
### paso 5: presentar el veredicto en el chat


Después de que la síntesis del presidente esté completa, presenta el veredicto completo directamente en el chat usando markdown. NO generes un reporte HTML ni ningún archivo. El usuario lo lee en la conversación.

Formatea el output así:
Mantenlo escaneable. Usa viñetas. Incluye los ejemplos antes/después donde sea relevante.


### paso 6: guardar la transcripción (opcional)


Solo guarda una transcripción si el usuario la pide o si la pregunta es lo suficientemente importante como para referenciarla después. Si la guardas, escribe en `council-transcript-[timestamp].md` en el directorio `active/` del proyecto.


---


## ejemplo: aconsejando una decisión de producto


**Usuario:** "Consejo esto: estoy pensando en crear un curso de $297 sobre Claude Code para principiantes. Mi audiencia son mayormente solopreneurs no técnicos. ¿Es este el movimiento correcto?"


**El Contrario:** "El mercado está inundado de cursos sobre Claude ahora mismo. A $297, estás compitiendo con contenido gratuito de YouTube. Tu audiencia es no técnica, lo que significa alta carga de soporte y riesgo de reembolsos. Las personas que pagarían $297 probablemente ya pasaron el nivel principiante..."


**El Pensador de Primeros Principios:** "¿Qué estás tratando de lograr realmente? Si es ingresos, un curso es uno de los caminos más lentos. Si es autoridad, un recurso gratuito podría hacer más. Si es construir una base de clientes para ofertas de mayor precio, el punto de precio y la audiencia pueden estar desalineados..."


**El Expansionista:** "Claude para solopreneurs principiantes es un mercado masivo desatendido. Todo el mundo está enseñando cosas avanzadas. Si clavas el ángulo principiante, eres dueño del punto de entrada a todo este espacio. Los $297 pueden ser muy bajos. ¿Y si esto se convirtiera en un programa de $997 con acceso a comunidad?"


**El Externo:** "No sé qué es Claude Code. Si viera 'curso de $297 sobre Claude Code para principiantes', no sabría si es para mí. El nombre no le dice nada a alguien fuera de tu mundo. Tu landing page necesita vender el resultado, no la herramienta..."


**El Ejecutor:** "Un curso completo toma 4-8 semanas producirlo correctamente. Antes de construir cualquier cosa, ejecuta un workshop en vivo a $97 con 50 personas. Validas demanda, generas testimonios y creas el material crudo para el curso. Si 50 personas no compran el workshop, 500 no comprarán el curso..."


**Veredicto del Presidente:**

*Dónde está de acuerdo el consejo:* El ángulo de solopreneur principiante tiene demanda real, pero el encuadre actual (curso de Claude Code) es demasiado específico a la herramienta y no resonará con compradores no técnicos.

*Dónde choca el consejo:* El precio. El Contrario dice que $297 es demasiado alto dada la competencia. El Expansionista dice que es demasiado bajo para el valor. La resolución probablemente depende de cuánto soporte y acceso a comunidad esté incluido.

*Puntos ciegos captados:* El punto del Externo de que "Claude Code" no le dice nada al comprador objetivo es el insight más importante. Todos los asesores excepto el Externo asumieron que la audiencia ya sabe qué es esto.

*Recomendación:* No construyas el curso todavía. Valida primero con una oferta de menor compromiso. Pero reformula completamente: vende el resultado (automatiza tu negocio, recupera 10 horas a la semana), no la herramienta.

*Lo primero que debes hacer:* Ejecuta un workshop en vivo de $97 llamado "Cómo automatizar tu primera tarea de negocio con IA" con 50 personas. No menciones Claude Code en el título.


---


## notas importantes


- **Siempre genera los 5 asesores en paralelo.** La generación secuencial desperdicia tiempo y deja que las respuestas anteriores contaminen las posteriores.
- **Siempre anonimiza para la revisión entre pares.** Si los revisores saben qué asesor dijo qué, deferirán a ciertos estilos de pensamiento en lugar de evaluar por mérito.
- **El presidente puede estar en desacuerdo con la mayoría.** Si 4 de 5 asesores dicen "hazlo" pero el razonamiento del disidente es más sólido, el presidente debe apoyar al disidente y explicar por qué.
- **No aconsejes preguntas triviales.** Si el usuario pregunta algo con una sola respuesta correcta, simplemente respóndela. El consejo es para incertidumbre genuina donde múltiples perspectivas agregan valor.
- **El reporte visual importa.** La mayoría de usuarios escanearán el reporte, no leerán la transcripción completa. Mantén el output HTML limpio y escaneable.
