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

### Cuadernos disponibles

- [`normalizar_autores_ollama_colab.ipynb`](normalizar_autores_ollama_colab.ipynb) — Reto 1.
- [`reto4_auditoria_marc21_colab.ipynb`](reto4_auditoria_marc21_colab.ipynb) — Reto 4.

**Retos 2 y 3:** utiliza las celdas de Colab correspondientes a esos ejercicios y agrega sus cuadernos `.ipynb` al repositorio cuando los exportes. Los archivos de datos para ambos retos ya están incluidos.

## Requisitos

- Una cuenta de [Google Colab](https://colab.research.google.com/).
- Python con `pandas`, `requests` y `openpyxl` (para los archivos Excel).
- [Ollama](https://ollama.com/) con el modelo **`llama3.2:1b`**.
- **Opcional, recomendado:** entorno de ejecución con GPU (por ejemplo, NVIDIA T4, si Colab la ofrece).

## Cómo comenzar en Google Colab

1. Abre el cuaderno `.ipynb` que deseas ejecutar desde GitHub o súbelo a Google Colab.
2. Si tienes la opción, selecciona **Entorno de ejecución → Cambiar tipo de entorno de ejecución → T4 GPU**.
3. En una sesión nueva de Colab, instala e inicia Ollama **una sola vez** y descarga el modelo. Puedes usar estas celdas:

```python
# Instalar Ollama (en una sesión nueva de Colab)
!apt-get update -qq
!apt-get install -y -qq zstd
!curl -fsSL https://ollama.com/install.sh | sh
```

```python
# Iniciar servidor y preparar el modelo
import subprocess
import time
import requests

log = open('/tmp/ollama.log', 'w')
servidor = subprocess.Popen(['ollama', 'serve'], stdout=log, stderr=subprocess.STDOUT)

for _ in range(20):
    try:
        if requests.get('http://127.0.0.1:11434/api/tags', timeout=2).ok:
            break
    except requests.RequestException:
        pass
    time.sleep(1)
else:
    raise RuntimeError('No se pudo iniciar Ollama')

subprocess.run(['ollama', 'pull', 'llama3.2:1b'], check=True)
print('Ollama preparado')
```

> Si **ya instalaste, iniciaste y descargaste** Ollama en esa misma sesión de Colab, **no repitas** estos pasos. Si Colab reinicia o elimina el entorno, puede ser necesario repetirlos.

4. Ejecuta el cuaderno del reto elegido y, cuando aparezca el selector de archivos, **sube el Excel o CSV correspondiente**.
5. Revisa las propuestas y descarga el archivo de resultados generado por el cuaderno.

Para comprobar dónde se ejecuta el modelo:

```python
!ollama ps
```

## Experimentos recomendados

En los ejemplos se usan los **primeros 10 registros** para reducir el tiempo de respuesta. Puedes cambiar el valor de `10` por `3`, `5` o `20` según la actividad y el tamaño del archivo:

```python
muestra = df.iloc[:10]  # primeros 10 registros
```

Para el **Reto 3**, puedes reutilizar el Excel resultante del **Reto 2**, siempre que conserves el resumen `520$a` y dejes vacía la columna `650$a` antes de generar nuevas materias.

Para las demostraciones se utiliza `llama3.2:1b` con opciones pensadas para respuestas breves:

```python
"keep_alive": "30m",
"options": {
    "temperature": 0,
    "num_predict": 150,
    "num_ctx": 1024
}
```

El límite de tokens debe adaptarse a la extensión de la respuesta: para generar resúmenes más largos, conviene aumentarlo.

## Precauciones de catalogación

- **Reto 1:** los nombres de persona deben cotejarse con los registros de autoridad; un modelo puede confundir o alterar apellidos.
- **Reto 2:** el resumen `520$a` debe describir el contenido respaldado por los metadatos disponibles, sin inventar resultados o conclusiones.
- **Reto 3:** las materias son **términos candidatos**. Antes de utilizarlos en MARC21, valida formas autorizadas, indicadores, subdivisiones y la política de la biblioteca. En el Excel se separan con `;` solo para facilitar la práctica.
- **Reto 4:** una alerta es una **posible incidencia**, no necesariamente un error confirmado. Las reglas son didácticas y no constituyen un validador MARC21 completo; el CSV original no se modifica.

**No uses datos personales de usuarios ni registros institucionales confidenciales en los ejercicios sin la autorización correspondiente.**

## Organización de archivos

Los nombres mostrados corresponden a los archivos de práctica preparados para este taller:

```text
README.md
normalizar_autores_ollama_colab.ipynb
reto4_auditoria_marc21_colab.ipynb
registros_bibliograficos_ia.xlsx
reto2_registros_marc21_resumenes.xlsx
reto3_registros_marc21_sin_temas.xlsx
reto4_registros_marc21_con_errores.csv
```

Puedes añadir los cuadernos de los retos 2 y 3 con los nombres que prefieras; después, incluye sus enlaces en la sección **Cuadernos disponibles**.

---

**Mensaje del taller:** la IA ayuda a proponer, enriquecer y revisar metadatos; **el criterio bibliotecario valida y decide**.
