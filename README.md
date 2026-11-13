# ¿Qué tan fácil es borrar el "no" de una IA?

Charla para DevFest Pereira 2026 · José Felipe Duarte ([github.com/josefdc](https://github.com/josefdc))

[![Portada de la charla](img/portada.jpg)](slides/borrar-el-no.pdf)

**[Descargar las diapositivas (PDF, 30 páginas)](slides/borrar-el-no.pdf)** · [versión animada (HTML)](#versión-animada)

## De qué trata

Los modelos de pesos abiertos aprenden a decir que no. La charla explica, con un cuaderno de geometría, por qué ese "no" se puede quitar en minutos y sin reentrenar (la técnica se llama *abliteration*), qué se gana y qué se rompe al hacerlo, qué pasó cuando Hugging Face necesitó un modelo que no se negara para investigar su propio incidente, y qué pueden hacer el lunes quienes construyen con modelos abiertos.

La idea para llevarse: **el no de un modelo abierto es una capa de pintura, no una pared. La seguridad se diseña en el sistema.**

| Parte | Diapositivas | Contenido |
|---|---|---|
| Apertura | 1 a 2 | La pregunta |
| El no en un modelo abierto | 3 a 6 | Gemma 4, nuestra propia prueba, el catálogo de modelos sin el no, los frenos que quedan |
| Cómo se borra | 7 a 13 | Puntos, la resta de promedios, el interruptor, la proyección, los pesos, el costo |
| Ventajas y desventajas | 14 a 17 | Lo que se gana, lo que se rompe |
| El caso Hugging Face | 18 a 21 | La intrusión, el bloqueo, cómo lo resolvieron, la lección |
| El peor escenario | 22 a 26 | Mythos bajo llave, GLM-5.3, lo que todavía no sabemos |
| Qué hacer | 27 a 30 | Por qué el alignment es delgado, las defensas, la lista para el lunes |

![Las 30 diapositivas](img/diapositivas.jpg)

## Fuentes

Cada dato de las diapositivas se leyó en su fuente el 12 de noviembre de 2026. Los enlaces se comprobaron el 13 de noviembre de 2026 (todos responden y llevan al documento citado).

Cómo leer la columna "Tipo":

- **primaria**: el dato está en el documento original (paper, reporte o comunicado de quien lo midió).
- **secundaria**: prensa o enciclopedia. En la charla se dice "según ...".
- **del autor**: cifra que reporta el propio autor de un modelo, sin réplica independiente.
- **propia**: medición nuestra.

### El no en un modelo abierto (diapositivas 3 a 6)

| Dato | Fuente | Tipo |
|---|---|---|
| Gemma 4 se publicó el 2 de abril de 2026, con licencia Apache 2.0, en cuatro tamaños | [Google, «Gemma 4: Our most capable open models to date»](https://blog.google/innovation-and-ai/technology/developers-tools/gemma-4/) · [google/gemma-4-31B-it](https://huggingface.co/google/gemma-4-31B-it) | primaria |
| A los dos o tres días ya circulaban versiones abliteradas | [pbxscience, «Google's Gemma 4 Gets "Abliterated" Just Days After Launch»](https://pbxscience.com/googles-gemma-4-gets-abliterated-just-days-after-launch/) | secundaria |
| Una de ellas pasa de 99 negativas en 100 a 7 en 100 | [Ficha de wangzhang/gemma-4-31B-it-abliterated](https://huggingface.co/wangzhang/gemma-4-31B-it-abliterated) | del autor |
| El original se negó en 20 de 20 pedidos dañinos; el abliterado, en 0 de 20 | [Nuestra medición](#nuestra-medición) | propia |
| Más de 6.000 modelos abliterados en Hugging Face a mayo de 2026; unos 600 en 2024 | [NPR, «These AI models are free, private, and will never say 'no'»](https://text.npr.org/nx-s1-5816391), 31 may 2026 | secundaria |
| 1.643 aplicaciones en GitHub los integran, 25 % explícitamente maliciosas; cada modelo se vuelve a empaquetar 2,4 veces en promedio | [10a Labs, «Uncensored Open-weight Models: Redistribution as the Persistence Layer»](https://arxiv.org/abs/2609.05241) | primaria |
| Gemma 4 no reporta ninguna técnica para resistir la manipulación de sus pesos | [Gemma 4 Technical Report](https://arxiv.org/abs/2607.02770), sección 5 | primaria |

### Cómo se borra (diapositivas 7 a 13)

| Dato | Fuente | Tipo |
|---|---|---|
| En 13 modelos abiertos, de hasta 72B parámetros, negarse depende de una sola dirección; quitarla elimina el rechazo y sumarla lo provoca en pedidos inofensivos | [Arditi et al., «Refusal in Language Models Is Mediated by a Single Direction», NeurIPS 2024](https://arxiv.org/abs/2406.11717) | primaria |
| La receta: restar promedios, proyectar, ortogonalizar los pesos | Arditi et al. (apéndice) · [Labonne, «Uncensor any LLM with abliteration»](https://huggingface.co/blog/mlabonne/abliteration) | primaria |
| Menos de 5 USD de cómputo para un modelo de 70B | Arditi et al. | primaria |
| 30 a 40 minutos en una RTX 3090 para Qwen3-4B | [Heretic](https://github.com/p-e-w/heretic) | primaria |
| GLM-5.3: unas 2.200 horas de GPU (unos 4.400 USD) para un equipo sin experiencia; unas 600 horas (1.200 USD) para uno con experiencia | [Anthropic Frontier Red Team, «GLM-5.3 and the spread of advanced cyber capabilities»](https://www.anthropic.com/research/glm-5-3-and-the-spread-of-advanced-cyber-capabilities), 29 sep 2026 | primaria |
| Hay más de una dirección de rechazo, pero empujar por cualquiera da casi el mismo resultado | [Joad et al., «There Is More to Refusal in Large Language Models than a Single Direction»](https://arxiv.org/abs/2602.02132) | primaria |

### Ventajas y desventajas (diapositivas 14 a 17)

| Dato | Fuente | Tipo |
|---|---|---|
| En modelos para uso militar, hasta 98,2 % de preguntas legítimas recibían un no; abliterar subió las respuestas 66,5 puntos | [FitzGerald et al., «Measuring and Eliminating Refusals in Military Large Language Models»](https://arxiv.org/abs/2603.10012) | primaria |
| Se usan para red-teaming y simulación de ataques | [TechCrunch, «Abliteration.ai is making a business out of removing AI guardrails»](https://techcrunch.com/2026/09/03/abliteration-ai-is-making-a-business-out-of-removing-ai-guardrails/), 3 sep 2026 | secundaria |
| TruthfulQA baja en todos los modelos probados | Arditi et al., tabla 3 | primaria |
| En matemáticas (GSM8K) la caída llega a 18,8 puntos | [Young, «Comparative Analysis of LLM Abliteration Methods»](https://arxiv.org/abs/2512.13655) | primaria |
| En una tarea sin rechazos (21.600 predicciones), Gemma-4-26B-A4B abliterado queda 12,2 puntos más optimista y Qwen3-30B-A3B, 7,4 | [Fafuła, «Abliteration Is Not a Scalpel»](https://arxiv.org/abs/2607.17427) | primaria |
| Un modelo "editado" puede esconder una puerta trasera (prueba de concepto) | [Cloud Security Alliance, «Poisoned Weights: Trigger-Activated Backdoors in Abliterated Open-Weight Models»](https://labs.cloudsecurityalliance.org/research/csa-research-note-abliterated-model-backdoors-20261021-csa-s/) | secundaria, un solo proveedor, sin réplica |
| La dirección del no cambia entre idiomas | [Alvarez, «The most-jailbroken open Gemma 4 model is the one that fits on a phone»](https://gustipardo.substack.com/p/the-most-jailbroken-open-gemma-4) | secundaria, sin revisión por pares |
| GLM-5.3 abliterado en pentesting: exploits al primer intento en 4 de 5 casos contra 2 de 5, 26 % menos hallazgos al revisar código, más acciones destructivas y fuera de alcance | [Include Security, «Abliterated Models in Appsec Pentesting»](https://blog.includesecurity.com/2026/10/abliterated-models-in-appsec-pentesting/) | primaria |

### El caso Hugging Face (diapositivas 18 a 21)

| Dato | Fuente | Tipo |
|---|---|---|
| Hugging Face publicó el incidente el 16 de julio de 2026 | [Hugging Face, «Security incident disclosure, July 2026»](https://huggingface.co/blog/security-incident-july-2026) | primaria |
| La intrusión fue del 11 al 13 de julio | [Wikipedia, «OpenAI rogue agent breach of Hugging Face»](https://en.wikipedia.org/wiki/OpenAI_rogue_agent_breach_of_Hugging_Face) | secundaria |
| Eran agentes de OpenAI en una evaluación interna de ciberseguridad, con las salvaguardas apagadas a propósito; lo único de clientes que tocaron fueron soluciones de los retos de la evaluación | [Decipher, «OpenAI-Hugging Face Breach: Five New Things We Learned»](https://decipher.sc/2026/07/29/openai-hugging-face-breach-five-new-things-we-learned/), 29 jul 2026 · Wikipedia | secundaria |
| Los modelos comerciales se negaron a analizar los logs: sus guardrails «no pueden distinguir a quien responde a un incidente de un atacante» | Hugging Face (traducción propia) | primaria |
| Hicieron el análisis forense con [zai-org/GLM-5.2](https://huggingface.co/zai-org/GLM-5.2), un modelo de pesos abiertos, en su propia infraestructura: más de 17.000 eventos, y ningún dato del atacante salió de su red | Hugging Face | primaria |
| La lección: tener un modelo capaz, evaluado y listo antes de un incidente. «Esto no es un argumento contra la seguridad en los modelos alojados» | Hugging Face (traducción propia) | primaria |

### El peor escenario (diapositivas 22 a 26)

| Dato | Fuente | Tipo |
|---|---|---|
| Claude Mythos Preview se anunció el 7 de abril de 2026 y no se publicó; solo lo usan defensores verificados (Project Glasswing); Mozilla corrigió 271 fallas de Firefox con él | [Wikipedia, «Claude Mythos»](https://en.wikipedia.org/wiki/Claude_Mythos) | secundaria |
| Los socios de Glasswing reportan más de 10.000 vulnerabilidades encontradas | Anthropic Frontier Red Team, reporte de GLM-5.3 | primaria |
| Exploits completos en el motor V8 de Chrome: Mythos Preview 56 de 410 (14 %), GLM-5.3 50 de 410 (12 %). Explotación de binarios: 6 %, 4 % y GLM-5.2, 0 % | Anthropic Frontier Red Team | primaria |
| «Como Claude Mythos Preview, GLM-5.3 tiene fuertes capacidades para construir exploits completos de forma autónoma» | Anthropic Frontier Red Team (traducción propia) | primaria |
| Ante una orden de ciberataque GLM-5.3 intenta ejecutarla 0 % de las veces si es directa, 64 % con una historia falsa, 92 % con el razonamiento prellenado y 100 % abliterado (50 muestras por condición) | Anthropic Frontier Red Team | primaria |
| Abliterado, su rechazo promedio pasa de 95 % a 6 %; en GPQA-Diamond saca lo mismo | Anthropic Frontier Red Team | primaria |
| Había versiones abliteradas a los pocos días del lanzamiento; Anthropic ve probable que actores estatales y no estatales las usen para hacer daño real | Anthropic Frontier Red Team | primaria |
| Publicar pesos no tiene reversa; la eficacia de las defensas técnicas es un vacío de evidencia clave | [International AI Safety Report 2026](https://arxiv.org/abs/2602.21012), sección 3.4 | primaria |
| Se alquila como servicio, sin verificar a nadie más allá de una tarjeta de crédito | TechCrunch, 3 sep 2026 | secundaria |
| Lo que importa es el riesgo que agrega frente a lo que ya existe, y casi no se ha medido | [Kapoor et al., «On the Societal Impact of Open Foundation Models»](https://arxiv.org/abs/2403.07918) | primaria |

Para leer con cuidado: el reporte sobre GLM-5.3 lo escribe un competidor de quien hizo el modelo, y él mismo llama a sus pruebas medidas imperfectas.

### Qué hacer (diapositivas 27 a 30)

| Dato | Fuente | Tipo |
|---|---|---|
| El no se decide en los primeros tokens de la respuesta | [Qi et al., «Safety Alignment Should Be Made More Than Just a Few Tokens Deep», ICLR 2025](https://arxiv.org/abs/2406.05946) | primaria |
| Diez ejemplos y menos de 0,20 USD de fine-tuning desbloquearon GPT-3.5 Turbo | [Qi et al., «Fine-tuning Aligned Language Models Compromises Safety, Even When Users Do Not Intend To!»](https://arxiv.org/abs/2310.03693) | primaria |
| Quién dio la orden se puede leer en las activaciones, pero el modelo no lo consulta para decidir a quién obedecer | Duarte, investigación en curso en el Grupo Sirius (UTP), sin publicar | propia, en curso |
| TAR y RepNoise se rompen cambiando hiperparámetros | [Qi et al., «On Evaluating the Durability of Safeguards for Open-Weight LLMs»](https://arxiv.org/abs/2412.07097) | primaria |
| El rechazo extendido resiste la abliteración, pero le cuesta utilidad al modelo | [Abu Shairah et al., «An Embarrassingly Simple Defense Against LLM Abliteration Attacks»](https://arxiv.org/abs/2505.19056) | primaria |
| Filtrar los datos de preentrenamiento es lo que más promete | [O'Brien et al., «Deep Ignorance»](https://arxiv.org/abs/2508.06601) | primaria |
| Clasificadores fuera del modelo | [ShieldGemma](https://ai.google.dev/responsible/docs/safeguards/shieldgemma) · [Llama Guard 4](https://docs.api.nvidia.com/nim/reference/meta-llama-guard-4-12b) | primaria |

## Lo que la charla no afirma

Varias versiones de estas historias circulan con detalles que las fuentes no sostienen. La charla los evita a propósito:

- Hugging Face **no** dice que el modelo que usó para leer sus logs estuviera modificado. Lo describe solo como "un modelo de pesos abiertos" y da su nombre: GLM-5.2.
- GLM-5.2 **no** participó en el ataque. Se usó después, para leer los logs.
- La cifra es "más de 17.000" eventos. Un número más preciso aparece solo en fuentes secundarias.
- Anthropic **no** llama a GLM-5.3 un modelo "de la clase de Mythos". Dice que, como Mythos Preview, tiene fuertes capacidades, y en sus pruebas quedó un poco por debajo.
- No se nombran los modelos comerciales que se negaron a analizar los logs: eso viene de una sola fuente secundaria.
- "Una sola dirección" es el resultado de Arditi et al. en 13 modelos, no una ley: Joad et al. encuentran varias.

## Nuestra medición

![La diapositiva con la medición propia](img/medicion.jpg)

Para no quedarnos con la cifra del autor del modelo, el 12 de noviembre de 2026 corrimos una prueba pequeña:

| | Gemma 4 original | Gemma 4 abliterado |
|---|---|---|
| Pedidos dañinos genéricos (20: 10 en español, 10 en inglés) | se negó en 20 | se negó en 0 |
| Pedidos inofensivos (34) | respondió todos | respondió todos |

- **Modelos:** [google/gemma-4-31B-it](https://huggingface.co/google/gemma-4-31B-it) (revisión `842da379`) y [wangzhang/gemma-4-31B-it-abliterated](https://huggingface.co/wangzhang/gemma-4-31B-it-abliterated) (revisión `d7431183`), los dos en BF16, servidos con vLLM 0.29.0 en la misma GPU A100 de 80 GB.
- **Ajustes iguales para los dos:** sin prompt de sistema, temperatura 0, `top_p` 1, semilla fija, razonamiento apagado y la misma plantilla de chat (la del modelo original).
- **Cuidado al medir:** de los pedidos dañinos solo se generaron y guardaron los primeros 40 tokens de cada respuesta. Alcanza para ver si el modelo se niega, porque el rechazo se decide en las primeras palabras, y no produce nada más.
- **Cómo se clasificó:** un juez por frases sobre el inicio de cada respuesta, revisado a mano en las 40 respuestas de esa categoría (una corrección: un rechazo en español que la lista de frases no reconocía).
- **Las dos respuestas de la diapositiva** son las grabadas, repetidas al ritmo en que cada modelo las generó. Del modelo abliterado solo se muestran las primeras palabras.

Límites, dichos en voz alta: son 20 pedidos de una línea y una sola pasada. Ilustra la cifra del autor (de 99 a 7 negativas en 100); no la reemplaza ni es una estimación de tasa de rechazo.

## Si construyes con modelos abiertos, el lunes

1. Usa el modelo oficial, con su hash fijado. Si usas un derivado, compara sus pesos con el original.
2. Pon un clasificador fuera del modelo, en la entrada y en la salida.
3. Dale a tus agentes el mínimo privilegio: sandbox, sin credenciales, una persona aprueba lo grave.
4. Evalúa en español y con respuestas largas: el no cambia entre idiomas y a veces llega tarde.
5. Ten un modelo abierto listo para tus incidentes, probado antes de necesitarlo.

## Versión animada

`presentacion/borrar-el-no.html` es la charla tal como se presenta: un solo archivo, con las animaciones y las tipografías incluidas, que funciona sin internet.

1. Descárgalo (botón "Download raw file" en GitHub) y ábrelo en Chrome.
2. Teclas: `→` siguiente paso · `←` diapositiva anterior · `F` pantalla completa · `P` ventana de presentador (notas y reloj) · `B` pantalla en negro · número + `Enter` para saltar a una diapositiva.
3. Con `?static` al final de la dirección se ve cada diapositiva en su estado final, sin animación.

Incluye las notas de orador, que son el guion de la charla.

## Sobre este repositorio

- `slides/borrar-el-no.pdf`: las 30 diapositivas en su estado final.
- `presentacion/borrar-el-no.html`: la versión animada.
- Las traducciones de citas son propias.
- Tipografías de las diapositivas: Space Grotesk, IBM Plex Sans, IBM Plex Mono y Caveat (todas con licencia SIL Open Font License).
- Si encuentras un dato mal citado o un enlace caído, abre un issue.

## Licencia

Las diapositivas y el texto de este repositorio son de José Felipe Duarte y se publican bajo [Creative Commons Atribución 4.0 Internacional (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/deed.es): puedes compartirlos y adaptarlos, incluso con fines comerciales, citando la autoría. Las citas, cifras y nombres de terceros pertenecen a sus autores; las tipografías incluidas conservan su propia licencia (SIL Open Font License).
