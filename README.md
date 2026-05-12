# LLM Council
Un skill de Claude que convierte una pregunta en cinco opiniones expertas, más un veredicto final.
Creado por [Ole Lehmann](https://x.com/itsolelehmann). Basado en la metodología [LLM Council](https://github.com/karpathy/llm-council) de Andrej Karpathy.
Funciona en **Claude Code** y **Claude Cowork** (claude.ai).
---
## *¿Qué es GitHub?*
Piensa en GitHub como Google Drive, pero para código. En vez de compartir fotos y archivos, la gente usa GitHub para compartir código y fragmentos como este skill. Ahora mismo estás en una página de GitHub.
---
## ¿Qué es un skill?
Un skill es un pequeño conjunto de instrucciones que le das a tu IA. Piénsalo como una descripción de trabajo para una tarea específica. Una vez instalado, puedes activarlo con una frase y Claude sabe exactamente cómo manejarlo.
Este skill en particular le enseña a Claude cómo ejecutar un "consejo" para ti. Cuando haces una pregunta difícil o enfrentas una decisión complicada, Claude activa 5 asesores diferentes que piensan desde ángulos fundamentalmente distintos, se revisan mutuamente y entregan una respuesta final sintetizada.
---
## Qué hace
Le haces una pregunta a una IA, obtienes una respuesta. Esa respuesta puede ser genial. Puede ser mediocre. No tienes forma de saberlo porque solo viste una perspectiva.
El consejo soluciona esto. Pasa tu pregunta por 5 asesores independientes, cada uno pensando desde un ángulo diferente. Se revisan entre sí. Un presidente sintetiza todo en una recomendación final que te dice dónde están de acuerdo, dónde chocan, y qué deberías hacer realmente.
---
## Cuándo usarlo
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
## Cómo instalarlo (sin terminal)
Elige la opción que te parezca más fácil. Ambas funcionan en Claude Code y Claude Cowork.
### Opción 1: Deja que Claude lo instale por ti
Abre un chat nuevo en Claude y pega esto:
> Por favor instala este skill de Claude para mí. El archivo SKILL.md está en este repositorio de GitHub: https://github.com/aiwithremy/claude-skills-llm-council
>
> Configúralo para que pueda empezar a usarlo. Guíame en lo que necesites de mi parte.
Claude buscará el archivo y lo pondrá en el lugar correcto. Si no puede hacerlo automáticamente (algunas configuraciones requieren subida manual), te dirá exactamente qué hacer.
### Opción 2: Descarga el archivo y pídele a Claude que lo configure
1. Haz clic en [SKILL.md](./SKILL.md) en la parte superior del repositorio.
2. Haz clic en el botón de descarga en el lado derecho para guardarlo en tu computador.
3. Abre Claude y pega esto:
> Acabo de descargar un archivo llamado SKILL.md para el skill LLM Council. ¿Puedes instalarlo para mí? Guíame en donde necesites que lo ponga.
Claude te guiará el resto del camino.
---
## Cómo usarlo
Una vez instalado, en cualquier conversación con Claude, simplemente di una de estas frases:
- "consejo esto"
- "ejecuta el consejo sobre [tu pregunta]"
- "ponlo a prueba"
- "estrésa esto"
- "guerra de ideas sobre esto"

Claude activará los 5 asesores, ejecutará la revisión entre pares y entregará el veredicto del presidente.
---
## Créditos
Este skill fue creado por [Ole Lehmann](https://x.com/itsolelehmann). Síguelo, el tipo la rompe.
La metodología está adaptada del [LLM Council](https://github.com/karpathy/llm-council) de Andrej Karpathy.
Este repositorio solo facilita compartirlo con amigos que quieran probarlo.
