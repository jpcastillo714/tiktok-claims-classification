# Propuesta de Proyecto: TikTok (Trust & Safety Track)

[⬅ Volver al README del Proyecto](README.md) | [Documento de Trabajo (tiktok.md)](tiktok.md) | [Protocolo de Agentes](AGENTS.md)

---

## 1. Visión General (*Overview*)

**TikTok** busca implementar soluciones de Machine Learning y Procesamiento de Lenguaje Natural para clasificar las afirmaciones contenidas en videos y comentarios reportados por usuarios, diferenciando con alta precisión entre **afirmaciones fácticas verificables (*claims*)** y **opiniones subjetivas (*opinions*)** para priorizar las colas de moderación de contenido.

---

## 2. Matriz de Hitos, Tareas y Partes Interesadas

Esta tabla establece el cronograma de trabajo acumulativo que abarca los seis cursos del programa:

| Hito (*Milestone*) | Tareas Principales | Etapa PACE | Entregables / Informes | Parte Interesada Relevante |
| :---: | :--- | :---: | :--- | :--- |
| **1** | Establecer la estructura del flujo de trabajo del proyecto | **Plan** | Documento global del proyecto (Estrategia PACE) | **Rosie Mae Bradshaw** — Responsable de Ciencia de Datos |
| **1a** | Redactar la propuesta de proyecto formal | **Plan** | Propuesta de proyecto aprobada | **Mary Joanna Rodgers** — Responsable de Gestión de Proyectos |
| **2** | Recopilar información resumida sobre los datos | **Analyze** | Archivos de datos (`tiktok_dataset.csv`) listos para EDA | **Orion Rainier** — Científico de Datos |
| **2a** | Iniciar la exploración preliminar de datos | **Analyze** | Resumen ejecutivo de calidad de datos | **Orion Rainier** — Científico de Datos |
| **3** | Exploración y limpieza profunda de datos | **Plan / Analyze** | Informe de resultados de EDA | **Orion Rainier** — Científico de Datos |
| **3a** | Construcción de cuadros de mando e informes visuales | **Analyze / Construct** | Cuadro de mando / visualizaciones en Tableau | **Mary Joanna Rodgers** — Responsable de Gestión de Proyectos |
| **4** | Calcular estadísticas descriptivas avanzadas | **Analyze** | Análisis de pruebas entre variables predictoras clave | **Willow Jaffey** — Jefa de Ciencia de Datos |
| **4a** | Realizar pruebas de hipótesis estadísticas | **Analyze / Construct** | Informe formal de pruebas de hipótesis | **Orion Rainier** — Científico de Datos |
| **5** | Construir modelo de regresión logística | **Analyze / Construct** | Modelo de clasificación binaria inicial | **Willow Jaffey** — Jefa de Ciencia de Datos |
| **5a** | Evaluar el modelo de regresión | **Execute** | Determinación del desempeño y matriz de confusión | **Rosie Mae Bradshaw** — Responsable de Ciencia de Datos |
| **6** | Construir modelos avanzados de Machine Learning | **Construct** | Modelo final (Random Forest / XGBoost) | **Willow Jaffey** — Jefa de Ciencia de Datos |
| **6a** | Comunicar los conocimientos finales a las partes interesadas | **Execute** | Informe ejecutivo integral y presentación a stakeholders | **Willow Jaffey** — Jefa de Ciencia de Datos |

---

## 3. Estimación de Tiempos Profesionales

- **Hito 1 (Planificación PACE y Propuesta):** 1 a 2 días.
- **Hito 2 (Estructuración e Ingesta de Datos):** 2 a 3 semanas.
- **Hito 3 (EDA y Visualización):** 1 semana.
- **Hito 4 (Estadística Inferencial):** 1 semana.
- **Hito 5 (Regresión y Modelado):** 1 a 2 semanas.
- **Hito 6 (Machine Learning y Cierre):** 2 semanas.
