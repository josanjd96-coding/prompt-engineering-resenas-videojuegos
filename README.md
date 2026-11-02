# prompt-engineering-resenas-videojuegos
Pipeline de dos LLMs (Groq) para filtrar reseñas de videojuegos y extraer información estructurada, con evaluación del prompt


# Análisis de reseñas de videojuegos con LLMs

Ejercicio de **Prompt Engineering** (Máster Data Analytics & Data Science, Pontia). Autor: José Antonio.

Pipeline de **dos LLMs** (Groq) que filtra reseñas de videojuegos sin información útil y extrae datos estructurados de las relevantes, incluida la predicción de si el autor recomienda el juego. La predicción se evalúa frente a la valoración real (`Recomendado_binario`).

## Contenido

| Fichero | Descripción |
|---|---|
| `Ejercicio 2.ipynb` | Notebook completo con el código, las explicaciones de cada paso, la evaluación y la justificación de decisiones |
| `resultado_extraccion.csv` | Resultado final: una fila por reseña relevante con la información extraída y la valoración real |
| `videogames_reviews.csv` | Dataset original (20.000 reseñas) |

## Pipeline

1. **Selección:** las 50 reseñas más largas; se quitan 12 duplicados exactos con pandas → 38.
2. **Filtrado (`openai/gpt-oss-20b`):** el primer LLM descarta las reseñas sin información útil (spam, texto sin sentido) → **37 relevantes**.
3. **Extracción (`openai/gpt-oss-120b`):** el segundo LLM extrae juego, aspectos positivos y negativos, dificultad, sentimiento y si recomienda el juego, en JSON con esquema estricto (Pydantic) y validación de cada respuesta.
4. **Evaluación:** la predicción del LLM frente a `Recomendado_binario` → **accuracy del 89,2 % (33 de 37)**, con tabla cruzada y análisis de los 4 fallos.

Las llamadas se hacen por lotes, con pausas calculadas según el límite de tokens por minuto y reintentos ante errores.

## Cómo ejecutarlo

1. Instalar las dependencias en el entorno de Python:

2. 2. Crear un fichero `.env` en la misma carpeta que el notebook con una API key de Groq (no se incluye en el repositorio):
  
3. 3. Abrir `Ejercicio 2.ipynb` y ejecutar todas las celdas en orden. La ejecución completa tarda unos 13 minutos por las pausas entre lotes.


