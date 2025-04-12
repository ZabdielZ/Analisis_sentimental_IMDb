# Analisis_sentimental_IMDb
# Avance 2: Limpieza y Procesamiento de Datos de Series IMDB

## 1. Introducción

Este documento describe los pasos seguidos para la recolección, limpieza y preparación de los datos de las 20 series mejor calificadas según IMDB. El objetivo fue generar un conjunto de datos estructurado que contuviera información relevante como título, rating, periodo de emisión, número de episodios y comentarios de usuarios.

---

## 2. Extracción de Datos

Para obtener los datos, se utilizó **web scraping con Selenium**. Los pasos principales fueron:

- Acceder al sitio IMDb Top 250 TV Shows.
- Identificar y extraer para cada serie los siguientes elementos:
  - Título de la serie.
  - URL del perfil principal de la serie.
  - Rating.
  - Años de emisión.
  - Número de episodios.
- A partir del enlace individual de cada serie, se construyó la URL de reseñas para extraer hasta 100 comentarios por serie.

---

## 3. Limpieza de Datos

### Valores nulos

- Se implementaron estructuras de control `try-except` durante la recolección para capturar casos donde ciertos datos no estuvieran disponibles (por ejemplo, si una serie no tiene rating visible o no tiene comentarios).
- En estos casos, se asignaron valores por defecto como `"Rating no disponible"` o `"Título no encontrado"` para evitar errores de ejecución y mantener la consistencia del dataset.

### Errores o datos incompletos

- Se identificaron casos donde no se logró extraer los 100 comentarios deseados. Esto puede deberse a que IMDb no tiene suficientes reseñas públicas disponibles para esa serie.
- No se forzó la inclusión artificial de comentarios adicionales.
- El contenido textual de los comentarios fue limpiado de manera básica (sin transformaciones adicionales) y se almacenó como lista de *strings*.

### Estructura del contenido

- Algunos campos como los comentarios se extrajeron directamente desde elementos HTML sin necesidad de limpieza adicional, dado que IMDb ya presenta los textos sin etiquetas incrustadas ni formatos problemáticos.

---

## 4. Procesamiento de Datos

### Normalización de campos

- Todos los valores de texto (título, rating, años, episodios, comentarios) se mantuvieron en formato de texto plano (*string*).
- No se aplicó transformación numérica a ratings o conteo de episodios, ya que eso se dejará para etapas posteriores del análisis.

### Almacenamiento estructurado

- Los datos fueron almacenados en una estructura tipo diccionario y luego convertidos a un `DataFrame` de pandas para facilitar análisis posteriores.
- Cada fila representa una serie distinta, con columnas para cada atributo: `titulo`, `link`, `rating`, `años_al_aire`, `episodios`, y `comentarios`.

---

## 5. Consideraciones Finales

- No se realizó una limpieza profunda de texto en los comentarios, ya que el objetivo de esta fase fue la extracción estructurada. Procesamiento de lenguaje natural (como eliminación de *stopwords* o análisis de sentimientos) se realizará en etapas futuras.
- No se detectaron valores duplicados, ya que se trabajó con una lista única de las 20 series top.

