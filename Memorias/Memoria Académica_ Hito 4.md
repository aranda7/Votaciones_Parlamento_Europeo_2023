# Memoria Académica: Hito 4 - Evaluación y Comparación de Modelos

## Introducción

Esta memoria documenta de manera descriptiva y académica el proceso y los resultados correspondientes al Hito 4 del proyecto. El objetivo de esta fase fue la evaluación final y la comparación exhaustiva de los cuatro modelos de clasificación (Regresión Logística, Random Forest, Gradient Boosting y Perceptrón Multicapa) después de su optimización en el Hito 3. El análisis se centra en el rendimiento de los modelos sobre el conjunto de datos de prueba, un análisis detallado de los errores y las propuestas de mejora derivadas de dicho análisis, basándose en los resultados consolidados en el notebook `Maestro.ipynb` y los archivos de datos asociados.



## 4.1. Evaluación y Comparación de Modelos en el Conjunto de Prueba

La evaluación final de los modelos optimizados se realizó sobre el conjunto de prueba, que no fue utilizado durante las fases de entrenamiento u optimización, garantizando así una medida insesgada de su capacidad de generalización. Todos los resultados, métricas y artefactos generados por los modelos individuales fueron centralizados y consolidados en el notebook `Maestro.ipynb` para facilitar un análisis comparativo directo y riguroso.

### 4.1.1. Comparación de Métricas de Rendimiento

Se realizó la comparación cuantitativa utilizando un conjunto de métricas de rendimiento globales y por clase. El archivo `Master.csv` y `Resultados_Datos_Entrenados.csv` compilan las métricas clave como **accuracy, precision, recall y F1-score** para cada modelo en los conjuntos de entrenamiento y prueba. Esto permitió no solo comparar los modelos entre sí, sino también evaluar el grado de sobreajuste al observar la diferencia de rendimiento entre ambos conjuntos.

Para una visualización más clara, se generaron tablas y gráficos comparativos. Por ejemplo, el archivo `AUC_Scores_Completo.csv` resume el rendimiento del Área Bajo la Curva (AUC) para cada clase y modelo, mientras que `ROC_Curves_Completo.csv` contiene los datos para graficar las curvas ROC de todos los modelos, permitiendo una comparación visual de su capacidad para discriminar entre clases.

### 4.1.2. Eficiencia Computacional

Además del rendimiento predictivo, se consideró la eficiencia computacional de cada modelo. El archivo `Tiempos_Computacionales.csv` registra los tiempos de entrenamiento y predicción para cada algoritmo, un factor relevante en entornos de producción donde la velocidad de respuesta puede ser crítica.



## 4.2. Análisis de Errores

Se llevó a cabo un análisis profundo de los errores de predicción con el fin de identificar patrones y comprender las debilidades de cada modelo. Este proceso es fundamental para proponer mejoras informadas.

### 4.2.1. Análisis de Matrices de Confusión

El análisis de las matrices de confusión, cuyos datos se consolidaron en el archivo `Matrices_Confusion.csv`, fue la principal herramienta para este propósito. Al examinar qué clases se confundían entre sí con mayor frecuencia, se pudieron identificar las áreas problemáticas específicas. Por ejemplo, se observó si los modelos tendían a confundir partidos políticos con ideologías similares o si las clases minoritarias eran sistemáticamente clasificadas incorrectamente como clases mayoritarias.

### 4.2.2. Métricas por Clase

Además de las matrices de confusión, se realizó un análisis detallado de las métricas de rendimiento para cada clase individual, como se recoge en `Metricas_Por_Clase.csv`. Esto permitió identificar con precisión qué clases eran las más difíciles de predecir para cada modelo, observando valores bajos de `precision` o `recall` para clases específicas, especialmente las minoritarias.

### 4.2.3. Propuestas de Mejora

Basándose en el análisis de errores, se propusieron varias vías para mejorar el rendimiento del modelo en futuras iteraciones:

*   **Mejoras en el Dataset:**
    *   **Ingeniería de Características (Feature Engineering):** Creación de nuevas variables que capten mejor las sutilezas entre las clases que se confunden con frecuencia.
    *   **Tratamiento Avanzado de Clases Minoritarias:** Explorar técnicas de sobremuestreo más sofisticadas que el oversampling estándar.
*   **Mejoras en la Arquitectura del Modelo:**
    *   **Ensamble de Modelos (Model Stacking/Blending):** Combinar las predicciones de los modelos más efectivas (por ejemplo, Random Forest y Gradient Boosting) para crear un meta-modelo que podría superar el rendimiento de cualquier modelo individual.
    *   **Ajuste Fino Específico:** Re-optimizar los hiperparámetros de un modelo centrándose específicamente en mejorar el rendimiento de las clases peor clasificadas.


Este análisis de errores no solo sirvió para evaluar el estado final de los modelos, sino que también proporcionó una hoja de ruta clara para el trabajo futuro en el proyecto.

