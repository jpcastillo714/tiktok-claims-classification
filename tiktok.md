# TikTok — Documento de Trabajo y Notas del Proyecto

[⬅ Volver al README del Proyecto](README.md) | [Propuesta del Proyecto (Hitos)](Propuesta_del_Proyecto_TikTok.md) | [Estrategia PACE](PACE_Strategy_Document_TikTok.md) | [Protocolo de Agentes](AGENTS.md)

---

## 1. Visión General y Escenario de Negocio

¡Bienvenido a tu nuevo rol en el equipo de datos de **TikTok**!

### Misión y Contexto
En TikTok, la misión corporativa es inspirar la creatividad y brindar alegría. La plataforma cuenta con una comunidad global de cientos de millones de creadores y usuarios activos que consumen y comparten videos cortos diariamente.

### Problema de Negocio
Los usuarios tienen la posibilidad de denunciar videos y comentarios que presuntamente infringen las directrices comunitarias. Dado el colosal volumen de reportes generados cada segundo, el equipo de moderación humana enfrenta un cuello de botella crítico. 

TikTok está desarrollando un **modelo predictivo de Machine Learning y PLN** para clasificar automáticamente si un video reportado contiene una **afirmación fáctica (*claim*)** o una **opinión personal (*opinion*)**:
- **Afirmación (*Claim*):** Declaración sobre hechos objetivos que puede ser contrastada y verificada empíricamente.
- **Opinión (*Opinion*):** Expresión de puntos de vista personales o creencias subjetivas.

Los videos clasificados como *claims* se priorizan inmediatamente para verificación humana especializada, reduciendo drásticamente la propagación de desinformación.

---

## 2. El Equipo del Proyecto y Partes Interesadas

### Equipo de Datos de TikTok (*Data Science Team*)
| Nombre | Rol | Responsabilidad Clave |
| :--- | :--- | :--- |
| **Willow Jaffey** | Jefa de Ciencia de Datos (*DS Lead*) | Dirección técnica general, definición de métricas clave y validación final de modelos. |
| **Rosie Mae Bradshaw** | Responsable de Ciencia de Datos (*DS Manager*) | Supervisora de equipo, asignación de proyectos y alineación con objetivos estratégicos. |
| **Orion Rainier** | Científico de Datos (*Data Scientist*) | Colega técnico y mentor, creador del Jupyter Notebook de inicio y análisis de datos. |

### Equipo Interfuncional (*Cross-Functional Team*)
| Nombre | Rol | Enfoque |
| :--- | :--- | :--- |
| **Mary Joanna Rodgers** | Responsable de Gestión de Proyectos (*PM Officer*) | Cronograma, coordinación de entregables y cuadros de mando en Tableau. |
| **Margery Adebowale** | Responsable de Finanzas, América | Asignación de recursos, costes de cómputo y presupuesto de moderación. |
| **Maika Abadi** | Jefe de Operaciones (*Operations Lead*) | Impacto operativo en la velocidad de moderación y flujos de trabajo de agentes humanos. |

---

## 3. Comunicaciones y Notas del Equipo

### Extracto de Notas de la Reunión de Liderazgo (Compartidas por Rosie Mae Bradshaw)
- **Mary Joanna Rodgers (PM Officer):** Se ha elaborado el marco global del proyecto con hitos alineados al flujo de trabajo PACE.
- **Orion Rainier (Data Scientist):** El dataset de reportes de TikTok debe examinarse minuciosamente antes de comenzar cualquier modelado.
- **Willow Jaffey (DS Lead):** El modelo predictivo permitirá reducir sustancialmente el retraso en la revisión de denuncias, priorizando las infracciones más graves.

---

### Correspondencia Interna por Correo Electrónico

#### Correo 1: Asignación de Rosie Mae Bradshaw
> **De:** "Bradshaw, Rosie Mae" `<rosiemaebradshaw@tiktok.com>`  
> **Para:** Nuevo Científico de Datos  
> **Cc:** "Rainier, Orion" `<orionrainier@tiktok.com>`  
> **Asunto:** ¿Ayuda con el cuaderno de codificación?  
>
> *Buenos días,*  
> *Acabamos de recibir la autorización para comenzar el proyecto de moderación de reportes. Antes de pasar al EDA completo, nos sería de gran ayuda que apoyes a Orion preparando e inspeccionando el cuaderno inicial de Python.*  
> *Mis más sinceros saludos,*  
> *Rosie Mae Bradshaw — Responsable de Ciencia de Datos, TikTok*

#### Correo 2: Indicaciones Técnicas de Orion Rainier
> **De:** "Rainier, Orion" `<orionrainier@tiktok.com>`  
> **Para:** Nuevo Científico de Datos  
> **Cc:** "Bradshaw, Rosie Mae" `<rosiemaebradshaw@tiktok.com>`  
> **Asunto:** RE: ¿Ayuda con el cuaderno de codificación?  
>
> *¡Encantado de conocerte (virtualmente)!*  
> *«Big data no va de bits, va de talento» — Douglas Merrill.*  
> *He preparado un Jupyter Notebook inicial para el dataset `tiktok_dataset.csv`. Por favor, ayúdanos a inspeccionar los datos: resume los tipos de datos (`dtypes`), cuenta valores no nulos, revisa la consistencia de las variables y detecta posibles inconsistencias.*  
> *Gracias,*  
> *Orion Rainier — Científico de Datos, TikTok*

---

## 4. Diccionario de Datos: `tiktok_dataset.csv`

El conjunto de datos contiene información sobre videos reportados y consta de **12 columnas**:

| Columna | Tipo de Dato | Descripción Oficial |
| :--- | :---: | :--- |
| `#` | Entero (*int*) | Número de índice numérico secuencial del registro. |
| `claim_status` | Objeto (*string*) | Estado de la afirmación: indica si el video fue etiquetado como `claim` (afirmación fáctica) u `opinion` (opinión). |
| `video_id` | Entero (*int*) | Identificador numérico único del video en la plataforma de TikTok. |
| `video_duration_sec` | Entero (*int*) | Duración total del video medida en segundos. |
| `video_transcription_text` | Objeto (*string*) | Transcripción en texto plano del audio del video. |
| `verified_status` | Objeto (*string*) | Estado de verificación de la cuenta del autor (`verified` = verificada, `not verified` = no verificada). |
| `author_ban_status` | Objeto (*string*) | Estado de sanción del autor: `active` (cuenta activa), `under scrutiny` (bajo revisión) o `banned` (suspendida). |
| `video_view_count` | Decimal (*float*) | Número total de reproducciones recibidas por el video. |
| `video_like_count` | Decimal (*float*) | Número total de «me gusta» (*likes*) acumulados por el video. |
| `video_share_count` | Decimal (*float*) | Número total de veces que el video fue compartido. |
| `video_download_count` | Decimal (*float*) | Número total de descargas del video realizadas por usuarios. |
| `video_comment_count` | Decimal (*float*) | Número total de comentarios publicados en el video. |

---

## 5. Tareas del Laboratorio y Entregables del Curso 1

1. **Cuaderno de Trabajo:** [`Activity_Course 2 TikTok project lab.ipynb`](Activity_Course%202%20TikTok%20project%20lab.ipynb)
   - Cargar `tiktok_dataset.csv` con `pandas`.
   - Inspeccionar dimensiones, tipos de datos y valores faltantes.
   - Evaluar la variable objetivo `claim_status` y analizar métricas de interacción (*views*, *likes*, *shares*).
2. **Documento de Estrategia PACE:** [`PACE_Strategy_Document_TikTok.md`](PACE_Strategy_Document_TikTok.md)
   - Responder las preguntas de las fases *Plan*, *Analyze* y *Execute*.
3. **Resumen Ejecutivo:**
   - Elaborar informe de 1 página con base en la [Plantilla de Resumen Ejecutivo](../Plantilla_Resumen_Ejecutivo.md).
