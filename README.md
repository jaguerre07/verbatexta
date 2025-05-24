# Análisis de Paisaje en "The Purple Land" de W.H. Hudson

Este proyecto utiliza un script de Python y técnicas de Procesamiento del Lenguaje Natural (PNL) para analizar la representación del paisaje y la naturaleza en la novela "The Purple Land" de W.H. Hudson, aunque puede ser utilizado en cualquier texto que se le ingrese y con los criterios que el usuario decida. El script identifica términos relevantes, ofrece sugerencias semánticas mediante un modelo Word2Vec entrenado localmente, y permite al usuario seleccionar interactivamente los términos para un análisis final de frecuencia y concordancias.

## Funcionalidades Principales

* Carga y preprocesa el texto de la novela.
* Realiza lematización asistida por etiquetado gramatical (POS tagging).
* Genera una lista de términos candidatos (sustantivos y adjetivos frecuentes) mediante POS tagging.
* Entrena un modelo Word2Vec localmente sobre el texto de la novela.
* Utiliza el modelo Word2Vec y un conjunto de palabras "semilla" (definibles por el usuario) para sugerir términos semánticamente relacionados con un concepto (ej. "paisaje y naturaleza").
* Permite al usuario ingresar interactivamente una lista final de términos a analizar, basándose en las sugerencias del POS tagging y Word2Vec.
* Calcula la frecuencia de los términos seleccionados por el usuario.
* Muestra concordancias (KWIC - Keyword in Context) para los términos seleccionados.

## Requisitos

1.  **Python 3:** Se recomienda Python 3.7 o superior.
2.  **Bibliotecas de Python:**
    * NLTK (`nltk`): Para tokenización, POS tagging, stopwords, lematización, etc.
    * Gensim (`gensim`): Para entrenar el modelo Word2Vec.
    Puedes instalarlas usando pip:
    ```bash
    pip3 install nltk gensim
    ```
3.  **Recursos de NLTK:**
    El script intentará usar los siguientes recursos de NLTK. Si no están presentes, NLTK podría intentar descargarlos o dar un error. Es recomendable descargarlos una vez ejecutando Python y luego los siguientes comandos:
    ```python
    import nltk
    nltk.download('punkt')         # Para tokenización de frases y palabras
    nltk.download('stopwords')     # Para la lista de palabras vacías (stopwords)
    nltk.download('averaged_perceptron_tagger') # Para el POS tagging
    nltk.download('wordnet')       # Para el WordNetLemmatizer
    nltk.download('omw-1.4')       # Open Multilingual Wordnet, necesario para WordNetLemmatizer
    ```
4.  **Archivo de Texto de la Novela:**
    * El script espera un archivo de texto plano (`.txt`) de "The Purple Land".
    * Actualmente, la ruta al archivo está codificada directamente en el script en la variable `file_path`. Deberás modificar esta línea para que apunte a la ubicación de tu archivo:
        ```python
        file_path = '/ruta/a/tu/archivo/purple_land_hudson.txt'
        ```

## Cómo Ejecutar el Script

1.  Asegúrate de tener todos los requisitos instalados.
2.  Guarda el script de Python (ej. `analizador_paisaje_hudson.py`) y el archivo de texto de la novela en tu computadora.
3.  Modifica la variable `file_path` dentro del script para que apunte a tu archivo de texto.
4.  Abre una terminal o línea de comandos.
5.  Navega hasta el directorio donde guardaste el script.
6.  Ejecuta el script con:
    ```bash
    python3 analizador_paisaje_hudson.py
    ```
7.  El script te guiará a través de varios pasos, imprimiendo información y listas de candidatos. Presta atención a los prompts interactivos:
    * **Selección de Palabras Semilla para Word2Vec (Paso 3.5):** Se te presentará una lista de palabras semilla por defecto para el concepto de "paisaje/naturaleza". Podrás elegir usar esta lista o ingresar la tuya propia (separada por comas).
    * **Selección Final de Términos para Análisis (Paso 4):** Después de ver los candidatos del POS tagging (Paso 3) y las sugerencias semánticas de Word2Vec (Paso 3.5), se te pedirá que ingreses la lista final de términos (separados por comas) que deseas analizar en detalle.

## Flujo de Trabajo del Script

El script realiza los siguientes pasos principales:

1.  **Paso 1: Carga del Archivo de Texto:** Lee el contenido de la novela desde el archivo especificado.
2.  **Paso 2: Preprocesamiento Básico General:**
    * Tokeniza el texto en palabras.
    * Convierte todos los tokens a minúsculas.
    * Elimina puntuación y stopwords para crear una lista de `tokens_processed`.
    * Realiza POS tagging sobre `tokens_processed` y luego lematiza estos tokens para crear `tokens_lemmatized`. Esta lista lematizada es la base para los análisis de frecuencia y concordancias.
    * Crea un objeto `nltk.Text` a partir de `tokens_lemmatized` para las concordancias.
3.  **Paso 2.1: Conteo de Frecuencia General:** Calcula y muestra las palabras más frecuentes en todo el texto (basado en `tokens_lemmatized`).
4.  **Paso 3: Generación de Candidatos por POS Tagging:**
    * Realiza POS tagging sobre `tokens_processed` (tokens limpios, pero no lematizados).
    * Filtra para quedarse con sustantivos y adjetivos.
    * Muestra los 50 sustantivos/adjetivos más frecuentes como una primera lista de candidatos (en sus formas originales, no lematizadas).
5.  **Paso 3.5: Filtrado Semántico Local con Word Embeddings (Gensim):**
    * Prepara el corpus para Word2Vec (oraciones tokenizadas del texto original, en minúsculas, solo palabras alfabéticas).
    * Entrena un modelo Word2Vec sobre este corpus.
    * Permite al usuario definir o usar una lista por defecto de **palabras semilla** representativas del concepto "paisaje/naturaleza".
    * Valida qué palabras semilla existen en el vocabulario del modelo Word2Vec.
    * Toma los N términos más frecuentes del corpus de Word2Vec como candidatos a filtrar.
    * Calcula la similitud (usando el modelo Word2Vec) entre cada término candidato y las palabras semilla válidas.
    * Presenta una lista de términos que superan un umbral de similitud predefinido.
6.  **Paso 4: Selección Interactiva de Términos por el Usuario:**
    * El usuario revisa las listas de candidatos del Paso 3 y las sugerencias del Paso 3.5.
    * Ingresa una lista final de términos de interés, separados por comas.
    * Estos términos ingresados son luego lematizados por el script.
7.  **Paso 5: Conteo de Frecuencia de Términos Seleccionados:** Calcula y muestra la frecuencia de cada término seleccionado por el usuario (ya lematizado) dentro del corpus lematizado (`tokens_lemmatized`).
8.  **Paso 6: Análisis de Concordancias:** Muestra las líneas de contexto donde aparecen los términos seleccionados por el usuario (lematizados) dentro del texto lematizado (`nltk_text`).

## Notas sobre el Modelo Word2Vec (Paso 3.5)

* El modelo Word2Vec se **entrena desde cero cada vez que se ejecuta el script**. Para corpus muy grandes, esto podría ser lento. Para una sola novela, debería ser razonablemente rápido.
* **Parámetros Clave para Experimentación:**
    * `vector_dim`: Dimensionalidad de los vectores de palabras (ej. 50-150 para un corpus pequeño).
    * `window_size`: Tamaño de la ventana de contexto (ej. 3-7).
    * `min_word_count`: Frecuencia mínima para que una palabra sea incluida en el vocabulario del modelo (ej. 2-5). Un valor bajo es importante para un corpus de una sola novela.
    * `sg_param`: Algoritmo de entrenamiento (0 para CBOW, 1 para Skip-gram).
    * `seed_words_nature_final`: La lista de palabras que definen tu concepto de "paisaje/naturaleza".
    * `similarity_threshold`: El umbral para considerar un término como semánticamente similar (ej. 0.5 - 0.7). Este valor requiere bastante experimentación.
* La calidad de las sugerencias semánticas dependerá de estos parámetros y de la especificidad del lenguaje en la novela.

## Personalización

* **Archivo de Texto:** Cambia la variable `file_path`.
* **Palabras Semilla:** Modifica la lista `default_seed_words_nature` en el script o proporciona tu propia lista interactivamente.
* **Parámetros de Word2Vec y Umbral de Similitud:** Ajusta estos valores directamente en la sección del "Paso 3.5" para refinar los resultados del filtrado semántico.

---

_Este README fue generado el 24 de mayo de 2025._