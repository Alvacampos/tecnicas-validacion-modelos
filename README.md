# Técnicas de diseño y validación de modelos

**Alumno:** Gonzalo Alvarez Campos

Examen de práctica (Evaluación Integradora) de la materia Técnicas de diseño y validación de modelos.

La Parte 1 trabaja con el dataset **AI4I 2020 Predictive Maintenance** (UCI): clasificación binaria de fallas de máquina con clases muy desbalanceadas. Compara un baseline (Dummy), Regresión Logística, Árbol de Decisión, Random Forest y Gradient Boosting con validación cruzada estratificada, usando métricas adecuadas al desbalance (Average Precision, recall, precisión), curvas de aprendizaje y `class_weight="balanced"`. El modelo final elegido es Gradient Boosting.

## Estructura del repositorio

| Archivo | Descripción |
|---|---|
| `Evaluacion Integradora - Parte 1 - Notebook (ejecutado).ipynb` | Notebook de la Parte 1 con las salidas ya ejecutadas. Incluye cuatro celdas "Anexo" agregadas (chequeo de leakage, pesos de clase, valores de la curva del Random Forest y curva de aprendizaje de Gradient Boosting). |
| `ai4i2020.csv` | Copia local del dataset (10.000 filas, 339 fallas), usada si la URL de UCI no responde. |
| `README.md` | Este archivo. |

## Cómo ejecutar

El notebook está pensado para **Google Colab** (también corre en Jupyter local con Python 3).

1. Abrir el notebook en Colab.
2. Ejecutar todas las celdas (*Entorno de ejecución → Ejecutar todas*).

Los datos se descargan desde la URL de UCI. Si no hay acceso, el notebook lee `ai4i2020.csv`: en Colab hay que subirlo al panel de archivos; en local, dejarlo en la misma carpeta que el notebook.

Dependencias: `numpy`, `pandas`, `matplotlib`, `scikit-learn` (ya vienen instaladas en Colab).

## Uso de herramientas de IA

Este trabajo se desarrolló con la ayuda de Claude (Anthropic), un asistente de IA. El notebook base es el provisto por la cátedra. El autor resolvió el examen, tomó las decisiones de diseño (entre ellas, la elección del modelo final) y revisó y aprobó cada paso. El asistente agregó y ejecutó las celdas "Anexo" del notebook y colaboró en la redacción de las respuestas, bajo la supervisión del autor, tomando como referencia el material de la materia.
