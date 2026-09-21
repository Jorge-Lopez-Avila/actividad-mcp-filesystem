# Evolución de los modelos de lenguaje



## ¿Qué es un modelo de lenguaje?



Un modelo de lenguaje (LM) es un sistema que calcula la probabilidad de que aparezca una palabra después de una secuencia dada. La idea viene de los modelos estadísticos clásicos como los n-gramas, donde se contaba cuántas veces aparecía cada palabra después de otra en un corpus.



Estos modelos tenían un problema: dependían completamente del contexto inmediato. Si intentabas predecir la siguiente palabra solo con las dos o tres anteriores, el resultado carecía de coherencia a nivel de párrafo o de idea completa.



## ¿Qué es un LLM?



Un LLM (Large Language Model) es un modelo de lenguaje entrenado con una arquitectura de red neuronal profunda, normalmente un transformer, sobre cantidades enormes de texto. La diferencia con los modelos estadísticos no es solo de escala: la arquitectura permite que el modelo atienda a contextos largos y capture relaciones entre palabras que están lejos en el texto.



Al aumentar el tamaño (más parámetros, más datos, más cómputo), aparecen capacidades que no estaban programadas explícitamente. Por ejemplo, traducir entre idiomas, resumir, resolver problemas matemáticos o escribir código. A esto se le llama capacidades emergentes.



Algunos ejemplos de LLM son GPT-4 y GPT-5 de OpenAI, Claude de Anthropic, Llama de Meta y Gemini de Google.



## Modelos con razonamiento explícito



A partir de 2024 apareció una nueva generación de modelos que piensan antes de responder. En lugar de dar la respuesta directamente, generan una cadena de razonamiento interna y luego emiten la conclusión.



Es importante aclarar algo que suele confundirse: esta capacidad no aparece sola por hacer el modelo más grande. Un modelo más grande es más capaz en general, pero el razonamiento explícito viene de dos cosas concretas:



1\. Técnicas de entrenamiento específicas, como el aprendizaje por refuerzo con retroalimentación humana (RLHF) o con verificadores automáticos (RLVR), donde el modelo es premiado por llegar a respuestas correctas razonando paso a paso.



2\. Cómputo adicional en el momento de la inferencia (test-time compute). En lugar de generar la respuesta en un solo paso, el modelo dedica más tokens a razonar antes de responder.



Ejemplos de modelos con razonamiento explícito son la serie o1 y o3 de OpenAI, DeepSeek-R1, y el modo extended thinking de Claude.



## Referencias



Anthropic. (2024). Claude's extended thinking. https://www.anthropic.com/research/extended-thinking



OpenAI. (2024). Learning to reason with LLMs. https://openai.com/index/learning-to-reason-with-llms/

