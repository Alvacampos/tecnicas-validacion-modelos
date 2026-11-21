# Examen de práctica · Parte 1 · Respuestas

Gonzalo Alvarez Campos · 20/11/2026

Cifras tomadas del Notebook N°1 ejecutado, sin usar el test. Las citas \[P1/P2, pág. N\] remiten a la página impresa de los apuntes p1 y p2.

## Consigna 1 · Comprensión del problema y desbalance de clases

**1.** El problema está desbalanceado. La celda A da una proporción de 0,9661 sin falla y 0,0339 con falla (9.661 contra 339 casos sobre 10.000). Es un desbalance severo, con menos del 10 % de positivos \[P1, pág. 66\], y por eso la partición es estratificada: 3,39 % de fallas en train y 3,40 % en test \[P1, pág. 29\].

**2.** La accuracy es (VP + VN) / total, así que la domina la clase mayoritaria \[P1, pág. 62\]. Un modelo que siempre predice “sin falla” acierta todos los negativos: el Dummy obtiene accuracy 0,966 con recall 0 y AP 0,034. En cambio, la Regresión Logística tiene menor accuracy (0,822) pero detecta el 80,5 % de las fallas. Con desbalance la accuracy sola es engañosa \[P1, pág. 62\], porque esconde el error que importa: la falla no detectada (FN).

**3.** Esas columnas indican qué tipo de falla ocurrió, de modo que solo se conocen después de la falla. Son variables derivadas del target, una forma típica de fuga de información \[P1, pág. 30\]. En el anexo del notebook, 330 de las 339 fallas tienen alguno de esos indicadores en 1, y 9.643 de los 9.661 casos sin falla los tienen todos en 0, así que el modelo copiaría la etiqueta. El resultado sería un modelo con métricas excelentes en validación que falla en producción, donde esa información no existe al momento de predecir \[P1, pág. 30\].

**4.** El parámetro actúa durante el entrenamiento: pondera por clase la función de pérdida que el modelo minimiza \[P1, pág. 24-25\].

```math
R_w = \frac{1}{n}\sum_{i=1}^{n} w_{y_i}\, L\big(y_i, f(x_i)\big), \qquad w_c = \frac{n}{k \cdot n_c}
```

Con el train, w₀ = 8.000 / (2 · 7.729) ≈ 0,518 y w₁ = 8.000 / (2 · 271) ≈ 14,76. Cada falla pesa unas 28,5 veces más, y así ambas clases aportan lo mismo a la pérdida. No modifica los datos ni el umbral. El efecto se ve en el recall: entre 0,805 y 0,878 en los modelos que lo usan, contra 0,590 en Gradient Boosting, que no lo admite.

## Consigna 2 · Comparación de modelos y selección justificada

| Modelo | Accuracy | Recall | Precision | ROC-AUC | Average Precision |
| --- | --- | --- | --- | --- | --- |
| Gradient Boosting | 0,983 | 0,590 | **0,861** | **0,975** | **0,790** |
| Random Forest | 0,943 | 0,860 | 0,357 | 0,967 | 0,655 |
| Árbol de Decisión | 0,927 | **0,878** | 0,301 | 0,889 | 0,516 |
| Regresión Logística | 0,822 | 0,805 | 0,138 | 0,897 | 0,443 |
| Dummy (mayoría) | 0,966 | 0,000 | 0,000 | 0,500 | 0,034 |

**a)** La mayor Average Precision es la de Gradient Boosting (0,790) y el mayor recall, el del Árbol de Decisión (0,878). No coinciden: GB tiene el recall más bajo (0,590) y el Árbol queda tercero en AP (0,516). El recall mide un solo umbral (0,5) e ignora las falsas alarmas, y el Árbol tiene una precisión de 0,301. La AP, en cambio, resume la curva Precision-Recall en todos los umbrales \[P1, pág. 72\].

**b)** La línea de base de la AP es la prevalencia de la clase positiva (0,034), no 0,5 como en ROC \[P1, pág. 66\]. Con tan pocas fallas, la FPR se mantiene baja aunque haya muchas falsas alarmas, y el ROC-AUC resulta optimista \[P1, pág. 65-66\]. En la tabla, los cuatro modelos tienen ROC-AUC entre 0,889 y 0,975, pero AP entre 0,443 y 0,790. Por ejemplo, la Regresión Logística tiene un ROC-AUC de 0,897 y aun así genera unas 6 falsas alarmas por cada falla detectada (precisión 0,138) \[P1, pág. 67\].

**c)** Elijo Gradient Boosting. El criterio es la capacidad de detectar la clase positiva medida con AP, la métrica primaria con desbalance severo \[P1, pág. 72\]. Tiene la mejor AP (0,790 contra 0,655 de Random Forest), el mejor ROC-AUC (0,975) y la mejor precisión (0,861). Su recall de 0,590 depende del umbral 0,5, que no suele ser el óptimo y puede ajustarse después con datos de validación \[P1, pág. 64\].

**d)** Como la Sección D grafica Random Forest, agregué al notebook la curva del modelo elegido con el mismo protocolo (anexo 4). En Gradient Boosting, la AP de entrenamiento va de 1,000 a 0,942 y la de validación, de 0,512 a 0,783, así que la brecha baja de 0,488 a 0,159. Es un sobreajuste moderado (alta varianza): entrenamiento alto, validación menor y una brecha que persiste \[P2, pág. 97\]. La brecha se reduce al aumentar los datos y la validación todavía sube en el último punto, por lo que más datos ayudarían, aunque no se cierra del todo \[P2, pág. 97 y 104\]. Random Forest (Sección D) muestra el mismo patrón con más brecha: de 0,513 a 0,295.

## Consigna 3 · Síntesis metodológica (206 palabras)

El problema es clasificar si una máquina CNC fallará a partir de cinco variables numéricas de sensores (temperaturas, rotación, torque y desgaste) y una categórica (tipo de producto). La falla es rara, con una prevalencia de 3,39 %, y el error más costoso es el falso negativo: una falla no anticipada.

Excluí TWF, HDF, PWF, OSF y RNF porque describen la falla ya ocurrida y generan fuga de información, junto con los identificadores UDI y Product ID. Usé un split 80/20 estratificado y validación cruzada estratificada de 5 folds para mantener la proporción de fallas en cada partición. El escalado y la codificación están dentro de un Pipeline, que se ajusta solo con los datos de entrenamiento de cada fold.

Elegí Gradient Boosting con Average Precision como métrica principal, porque su línea de base es la prevalencia y no se infla con los negativos fáciles. Obtuvo AP = 0,790 en validación cruzada, contra 0,655 de Random Forest y 0,034 del modelo trivial.

La limitación principal es su recall de 0,590 con el umbral por defecto, además de una brecha entre entrenamiento y validación de 0,159. Para superarla, ajustaría el umbral con datos de validación según el costo de cada error y probaría regularización o más datos.

*Respaldo:* \[P1, pág. 29, 30, 32-33, 59, 63, 64, 66 y 72\]; \[P2, pág. 97 y 104\].
