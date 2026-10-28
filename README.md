# Taller práctico: inteligencia artificial aplicada a la catalogación bibliográfica

**Cuatro retos con Google Colab, Python, Ollama y metadatos MARC21**

Este repositorio reúne ejercicios guiados para explorar cómo la inteligencia artificial puede apoyar el trabajo bibliotecario: **normalización de autores, generación de resúmenes, propuesta de materias y auditoría de metadatos**.

El objetivo es que cada participante cargue un archivo de práctica, seleccione unos pocos registros, pruebe un modelo de lenguaje local y descargue los resultados para revisarlos.

> **Uso educativo.** Los registros de ejemplo son ficticios o preparados para la práctica. Las respuestas de IA son propuestas: **no sustituyen la catalogación profesional ni la verificación en ficheros de autoridad y vocabularios controlados**.

## Retos del taller

| Reto | Actividad | Archivo de entrada | Resultado esperado |
|---|---|---|---|
| **1. Normalización de autores** | Proponer encabezamientos en formato «Apellidos, Nombres» | [`registros_bibliograficos_ia.xlsx`](registros_bibliograficos_ia.xlsx) | Excel con autores normalizados y columna del valor original |
| **2. Resúmenes bibliográficos** | Redactar propuestas para `520$a` a partir de título, índice, notas y temas | [`reto2_registros_marc21_resumenes.xlsx`](reto2_registros_marc21_resumenes.xlsx) | Excel con propuestas de resumen |
| **3. Puntos de acceso temáticos** | Proponer materias para `650$a` a partir del título, índice y resumen | [`reto3_registros_marc21_sin_temas.xlsx`](reto3_registros_marc21_sin_temas.xlsx) | Excel con materias candidatas |
| **4. Auditoría MARC21** | Detectar inconsistencias con reglas de Python y sugerir revisiones con IA | [`reto4_registros_marc21_con_errores.csv`](reto4_registros_marc21_con_errores.csv) | CSV con incidencias y recomendaciones |


## Requisitos

- Una cuenta de [Google Colab](https://colab.research.google.com/).
- Python con `pandas`, `requests` y `openpyxl` (para los archivos Excel).
- [Ollama](https://ollama.com/) con el modelo **`llama3.2:1b`**.
- **Opcional, recomendado:** entorno de ejecución con GPU (por ejemplo, NVIDIA T4, si Colab la ofrece).

## Cómo comenzar en Google Colab

1. Abre el cuaderno `.ipynb` que deseas ejecutar desde GitHub o súbelo a Google Colab.
2. Si tienes la opción, selecciona **Entorno de ejecución → Cambiar tipo de entorno de ejecución → T4 GPU**.
3. En una sesión nueva de Colab, instala e inicia Ollama **una sola vez** y descarga el modelo. Puedes usar estas celdas:


**Mensaje del taller:** la IA ayuda a proponer, enriquecer y revisar metadatos; **el criterio bibliotecario valida y decide**.
