# Técnicas de diseño y validación de modelos

**Alumno:** Gonzalo Alvarez Campos

Evaluación Integradora de la materia Técnicas de diseño y validación de modelos.

La Parte 1 trabaja con el dataset **AI4I 2020 Predictive Maintenance** (UCI): clasificación binaria de fallas de máquina con clases muy desbalanceadas. Compara un baseline (Dummy), Regresión Logística, Árbol de Decisión, Random Forest y Gradient Boosting con validación cruzada estratificada, usando métricas adecuadas al desbalance (Average Precision, recall, precisión), curvas de aprendizaje y `class_weight="balanced"`. El modelo final elegido es Gradient Boosting.

La Parte 2 evalúa ese modelo una sola vez sobre el conjunto de test (matriz de confusión, coherencia con la validación cruzada), analiza el umbral de decisión según el costo de los falsos negativos (se elige 0,20) y cierra con una conclusión integradora.

## Estructura del repositorio

| Archivo | Descripción |
|---|---|
| `Evaluacion_Integradora_Parte_1_Notebook.ipynb` | Notebook de la Parte 1 (Colab) con las salidas ya ejecutadas. Incluye cuatro celdas "Anexo" agregadas (chequeo de leakage, pesos de clase, valores de la curva del Random Forest y curva de aprendizaje de Gradient Boosting). |
| `Evaluacion_Integradora_Parte_2_Notebook_(ejecutado).ipynb` | Notebook de la Parte 2 (Colab) ejecutado, con las respuestas y la conclusión integradora escritas en el propio notebook. |
| `Parte 1 - Respuestas.md` | Respuestas a las consignas de la Parte 1, con las cifras del notebook y citas a los apuntes de la materia. |
| `README.md` | Este archivo. |

## Cómo ejecutar

Los notebooks están pensados para **Google Colab** (también corren en Jupyter local con Python 3).

1. Abrir el notebook en Colab. La Parte 2 vuelve a cargar los datos y se puede correr sola.
2. Ejecutar todas las celdas (*Entorno de ejecución → Ejecutar todas*).

Los datos se descargan desde la URL de UCI. Si no hay acceso, cada notebook lee una copia local `ai4i2020.csv` (no incluida en el repo): en Colab hay que subirla al panel de archivos; en local, dejarla en la misma carpeta que los notebooks.

Dependencias: `numpy`, `pandas`, `matplotlib`, `scikit-learn` (ya vienen instaladas en Colab).

## Uso de herramientas de IA

Este trabajo se desarrolló con la ayuda de Claude (Anthropic), un asistente de IA. Los notebooks base son los provistos por la cátedra. El autor resolvió el examen, tomó las decisiones de diseño (entre ellas, la elección del modelo final) y revisó y aprobó cada paso. El asistente agregó y ejecutó las celdas "Anexo" de la Parte 1, adaptó la celda del modelo final de la Parte 2 a Gradient Boosting y colaboró en la redacción de las respuestas, bajo la supervisión del autor, tomando como referencia el material de la materia.
