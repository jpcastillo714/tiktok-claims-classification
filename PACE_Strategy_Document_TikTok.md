# Documento de Estrategia PACE: TikTok (Trust & Safety Track)

[⬅ Volver al README del Proyecto](README.md) | [Documento de Trabajo (tiktok.md)](tiktok.md) | [Propuesta del Proyecto](Propuesta_del_Proyecto_TikTok.md) | [Resumen Ejecutivo](Resumen_Ejecutivo_TikTok_Curso1.md)

Este documento registra las decisiones y reflexiones analíticas para el proyecto TikTok siguiendo las 4 fases del marco PACE.

---

## Plan

### 1. ¿Cómo puedes prepararte mejor para comprender y organizar la información proporcionada sobre el negocio y el dataset?
- *Respuesta:*
  - **Revisión del Diccionario de Datos:** Estudiar exhaustivamente la especificación de las 12 columnas en [tiktok.md](tiktok.md) para comprender la escala, tipo de datos y rol de cada variable en el negocio.
  - **Alineación de Objetivos con Partes Interesadas:** Coordinar con Willow Jaffey (DS Lead), Rosie Mae Bradshaw (DS Manager) y Mary Joanna Rodgers (PM Officer) para garantizar que el modelo predictivo resuelva el cuello de botella operativo de *Trust & Safety*, distinguiendo entre afirmaciones fácticas (*claims*) y opiniones (*opinions*) para priorizar la revisión humana.
  - **Comprensión del Ciclo de Moderación:** Analizar la trayectoria de un video reportado: cómo ingresa al sistema, cómo el triaje automático asigna prioridad a las denuncias y cómo impacta en las decisiones sobre el estado de la cuenta del creador (`author_ban_status`).
  - **Organización del Entorno de Datos:** Configurar un entorno reproducible en Python con `pandas` y `numpy` para estructurar la ingesta, limpieza, análisis exploratorio preliminar y documentación continua bajo el marco PACE.

### 2. ¿Qué recursos, guías o documentación de consulta te ayudarán a ejecutar este trabajo?
- *Respuesta:*
  - **Normas Comunitarias de TikTok:** Directrices oficiales sobre desinformación cívica, seguridad pública, autenticidad y salud para entender qué criterios legales y de confianza rigen una infracción fáctica.
  - **Apuntes Teóricos del Curso 1:** [Marco PACE en Profundidad](../../01_Foundations_of_Data_Science/apuntes/Modulo_4_Comunicacion_Efectiva_y_Marco_PACE.md) para gobernar las cuatro etapas analíticas y [Estrategia de Portafolio](../../01_Foundations_of_Data_Science/apuntes/Modulo_5_Estrategia_de_Portafolio_y_Proyectos.md) para alinear los entregables ejecutivos.
  - **Documentación Técnica del Stack de Python:** Manuales de referencia de `pandas` y `numpy` para la manipulación y agregación de DataFrames.
  - **Glosario de Términos Clave:** [Glosario](../../01_Foundations_of_Data_Science/apuntes/Glosario_Terminos_Clave.md) para mantener consistencia en la terminología analítica y métricas de rendimiento.

### 3. ¿Qué actividades adicionales realizaría un profesional de datos proactivo antes de comenzar a escribir código?
- *Respuesta:*
  - **Entrevistas con Operaciones de Moderación:** Consultar con Maika Abadi (Operations Lead) para estimar los tiempos medios de respuesta (SLA) de los moderadores humanos y calcular el coste que representan los reportes clasificados erróneamente.
  - **Evaluación Ética y Mitigación de Sesgo Algorítmico:** Analizar si el modelo preliminar podría discriminar inadvertidamente contra cuentas no verificadas (`not verified`) o con dialectos específicos, garantizando el respeto al principio de equidad algorítmica (*algorithmic fairness*).
  - **Auditoría Preliminar de Calidad de Datos:** Identificar si existen duplicados por `video_id`, evaluar la sincronía de registros nulos y comprobar la consistencia de las transcripciones de audio.
  - **Estrategia Anticipada de Métricas de Evaluación:** Definir tempranamente que, para el problema de *Trust & Safety*, la métrica crítica a maximizar en futuros modelos debe ser el **Recall (Exhaustividad)** sobre la clase `claim`, ya que el coste de un Falso Negativo (un video con desinformación peligrosa que no se modera) es significativamente mayor que el de un Falso Positivo (revisar una opinión que resulta inocua).

---

## Analyze

### 1. ¿La información disponible en el dataset parece suficiente para alcanzar el objetivo comercial según tu intuición y el análisis preliminar de las variables?
- *Respuesta:*
  - Sí, el conjunto de datos de 19,382 filas y 12 columnas proporciona una base empírica sumamente rica y discriminante para clasificar afirmaciones vs opiniones:
    - **Señales de Interacción Cuantitativa:** Las variables de engagement muestran una brecha de magnitud colosal: los videos con `claim` reciben en promedio ~501,000 visualizaciones, mientras que los videos con `opinion` promedian apenas ~4,956 visualizaciones (una diferencia de 100 a 1).
    - **Asociación con Sanciones de Autor:** El 88.0% de los videos de autores suspendidos (`banned`) y el 77.6% de autores bajo escrutinio (`under review`) corresponden a *claims*, lo que constituye un predictor categórico de alto impacto.
    - **Contenido Semántico:** La columna `video_transcription_text` contiene el texto explícito que en cursos posteriores permitirá entrenar modelos de procesamiento de lenguaje natural (NLP: TF-IDF, embeddings y clasificadores avanzados).
    - **Limitación Identificada:** Existen 298 registros con valores nulos sincronizados en la variable objetivo y las métricas de interacción, los cuales deben aislarse en el preprocesamiento.

### 2. ¿Cómo construirías estadísticas descriptivas del DataFrame (`df.describe()`, `df.info()`) y evaluarías el rango mínimo y máximo de las variables clave?
- *Respuesta:*
  - Ejecutar `data.info()` para auditar los tipos de datos (5 `float64`, 3 `int64`, 4 `object`) y confirmar el volumen de no nulos por columna.
  - Generar la tabla de resumen estadístico mediante `data.describe()` para examinar la tendencia central (media vs mediana) y dispersión (desviación estándar y percentiles 25%, 50%, 75%).
  - Evaluar la coherencia de rangos:
    - `video_duration_sec`: Mínimo de 5 s y máximo de 60 s (media de 32.4 s), coherente con el formato de videos cortos de TikTok y sin valores ilógicos.
    - Métricas de interacción: Todas tienen un límite inferior de 0 (confirmando ausencia de conteos negativos). Los máximos alcanzan 999,813 vistas, 657,830 likes, 256,481 shares, 14,994 descargas y 9,599 comentarios.
  - Diseñar variables relativas (`likes_per_view`, `comments_per_view`, `shares_per_view`) para medir la intensidad del engagement con independencia del volumen bruto.

### 3. ¿Los promedios o distribuciones de alguna de las variables numéricas parecen inusuales o inconsistentes? ¿Cómo describirías las variables de intervalo o temporales?
- *Respuesta:*
  - **Distribución Bimodal Oculta:** Globalmente, las visualizaciones parecen tener una asimetría extrema hacia la derecha (media de 254,708 vs mediana de 9,954). Sin embargo, al estratificar por `claim_status`, la distribución se descompone en dos modas netamente diferenciadas pero homogéneas internamente:
    - `claim`: Media = 501,029.45 | Mediana = 501,555.00
    - `opinion`: Media = 4,956.43 | Mediana = 4,953.00
  - **Comportamiento Viral Anómalo en Cuentas Sancionadas:** Los autores suspendidos tienen una mediana de compartidos de **14,468**, en comparación con **437** para autores activos (más de 33 veces superior). Esto evidencia que el contenido que termina siendo sancionado logra una viralización masiva antes de ser intervenido por moderación.
  - **Variables Temporales y de Duración:** La duración del video (`video_duration_sec`) es una variable de razón acotada entre 5 y 60 segundos, distribuida de forma uniforme y simétrica (media 32.4 s, mediana 32.0 s), sin mostrar correlación directa con la veracidad del contenido.

---

## Construct

> [!NOTE]
> La fase Construct no aplica para el Curso 1. Se abordará en los cursos de Regresión (Curso 4) y Machine Learning (Curso 5).

---

## Execute

### 1. Dado tu conocimiento actual de los datos, ¿qué recomendarías inicialmente a tu responsable directo que investigue más a fondo antes de pasar al análisis exploratorio profundo (EDA)?
- *Respuesta:*
  - **Origen de los 298 Registros Faltantes:** Solicitar al equipo de ingeniería de datos que aclare el origen de los 298 registros con valores nulos simultáneos en `claim_status`, transcripción y todas las métricas de interacción. Es clave verificar si corresponden a videos borrados por el usuario, fallos de telemetría o contenido retirado de urgencia.
  - **Protocolo Anti-Sesgo hacia Creadores:** Advertir a Rosie Mae Bradshaw que un modelo que sobre-pondere el estado de suspensión (`author_ban_status`) podría incurrir en sesgos de bucle de retroalimentación (*feedback loop bias*), penalizando sistemáticamente a ciertos autores. El modelo debe fundamentarse en el contenido del video y no únicamente en el historial del autor.
  - **Estrategia para el EDA del Curso 2:** Preparar visualizaciones multivariadas en Tableau/Seaborn y planificar el análisis exploratorio de la longitud del texto transcrito y palabras clave frecuentes.

### 2. ¿Qué datos o variables presentan inicialmente señales de contener anomalías, valores negativos o registros atípicos?
- *Respuesta:*
  - **Valores Negativos:** Ninguno. Todos los conteos inician en 0 y las duraciones en 5 segundos.
  - **Valores Atípicos (Outliers Extremos):** Se aprecian picos virales extremos en compartidos (hasta 256,481) y me gusta (hasta 657,830), predominantemente en videos de autores bajo revisión o suspendidos. Se requerirán transformaciones logarítmicas o escaladores robustos en la etapa de modelado.
  - **Patrón de Faltantes No Aleatorios (MNAR):** La sincronía exacta de los 298 valores nulos confirma que no es una pérdida aleatoria, sino un subconjunto estructuralmente incompleto que debe descartarse del entrenamiento supervisado.

### 3. ¿Qué tipos de datos adicionales o fuentes externas podrían enriquecer y fortalecer este conjunto de datos para los siguientes cursos?
- *Respuesta:*
  - **Categoría / Motivo del Reporte de Usuario:** Detalle de la razón señalada por el usuario al reportar (desinformación médica, estafa, violencia, spam, etc.).
  - **Métricas de Velocidad y Temporalidad:** Marcas de tiempo de publicación y de reportes. Calcular la velocidad de visualizaciones por hora (*velocity*) proporcionaría una señal crítica para el triaje en tiempo real.
  - **Historial y Reputación del Creador:** Antigüedad de la cuenta, número total de seguidores, volumen histórico de videos publicados y proporción de advertencias previas.
  - **Señales Audiovisuales y Texto en Pantalla:** Detección de texto superpuesto (*on-screen text / OCR*), etiquetas de hashtags (`#tags`) y atributos acústicos de la pista de audio.
