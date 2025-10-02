# Memoria Académica: Hitos 2 y 3

## Introducción

Esta memoria describe de manera objetiva y académica el trabajo realizado en los Hitos 2 y 3 del proyecto, centrados en el entrenamiento, evaluación y optimización de los modelos de clasificación para la predicción de la intención de voto. El documento se estructura en dos secciones principales, correspondiendo a cada uno de los hitos y presenta los resultados obtenidos.


## Hito 2: Entrenamiento Inicial del Modelo

En esta fase, se realizó el entrenamiento inicial de los cuatro modelos seleccionados (Regresión Logística, Random Forest, Gradient Boosting y Perceptrón Multicapa) utilizando sus parámetros por defecto. El objetivo fue establecer una línea de base de rendimiento para cada uno y realizar una primera comparación.

### 2.1. Metodología de Entrenamiento

Para todos los modelos, el proceso de entrenamiento siguió una metodología común. En primer lugar, se identificó un desbalance significativo en las clases de la variable objetivo en el conjunto de entrenamiento. Para mitigar el posible sesgo del modelo hacia las clases mayoritarias, se aplicó la técnica de sobremuestreo **Over-sampling Simple)** únicamente al conjunto de entrenamiento. El conjunto de prueba se mantuvo en su estado original para garantizar una evaluación realista del rendimiento del modelo en datos no vistos.

El entrenamiento se ejecutó de forma individual para cada modelo, como se documenta en sus respectivos notebooks (`RL.ipynb`, `RF.ipynb`, `GB.ipynb`, `MLP.ipynb`). Se registraron las métricas de rendimiento tanto para el conjunto de entrenamiento sobremuestreado como para el conjunto de prueba original.

### 2.2. Resultados del Entrenamiento Base

Los resultados del entrenamiento inicial con parámetros por defecto permitieron obtener una primera impresión del comportamiento de cada algoritmo. Se evaluaron métricas globales como `accuracy`, `precision`, `recall` y `F1-score`, y se generaron visualizaciones como matrices de confusión y curvas ROC para un análisis más detallado.

Por ejemplo, en el entrenamiento inicial del modelo MLP, se obtuvo una matriz de confusión que muestra el número de predicciones correctas e incorrectas para cada clase en el conjunto de prueba, como se observa en la figura `matriz_confusion_default.png`. De manera similar, las curvas ROC, visibles en `roc_auc_default.png`, ilustraron la capacidad del modelo para discriminar entre las diferentes clases.

Esta fase concluyó con la obtención de un conjunto de métricas y resultados de línea de base para cada uno de los cuatro modelos, lo que sirvió como punto de partida para la fase de optimización. Asimismo se abordó la problematica de la Clase 6.



## Hito 3: Optimización del Modelo

El objetivo de este hito fue mejorar el rendimiento de los modelos base mediante el ajuste de hiperparámetros y la aplicación de técnicas de regularización para mejorar la generalización y evitar el sobreajuste.

### 3.1. Ajuste de Hiperparámetros

Para cada uno de los modelos, se llevó a cabo un proceso de optimización de hiperparámetros. La técnica principal utilizada fue la búsqueda en rejilla con validación cruzada (`GridSearchCV`). Se definió un espacio de búsqueda de hiperparámetros para cada algoritmo, cubriendo aquellos que tienen un mayor impacto en el rendimiento del modelo, como la tasa de aprendizaje, el número de estimadores, la profundidad de los árboles o los parámetros de regularización.

El proceso de `GridSearchCV` se entrenó utilizando el conjunto de entrenamiento sobremuestreado (`X_train_oversampled`, `Y_train_oversampled`) para encontrar la combinación de hiperparámetros que maximizaba una métrica de rendimiento específica (generalmente, el `F1-score` ponderado, debido al desbalance de clases original).

Una vez encontrados los mejores hiperparámetros, se reentrenó el modelo con esta configuración óptima y se evaluó su rendimiento final sobre el conjunto de prueba no modificado. Forzando, también, la presencia de la clase 6.

### 3.2. Regularización y Generalización

Se prestaron esfuerzos para controlar el sobreajuste (overfitting), que ocurre cuando un modelo aprende el ruido del conjunto de entrenamiento en lugar de los patrones subyacentes, lo que lleva a un bajo rendimiento en datos nuevos. La principal estrategia para identificar el sobreajuste fue comparar las métricas de rendimiento entre el conjunto de entrenamiento y el de prueba. Una brecha significativa entre ambas indicaría un posible sobreajuste.

Se aplicaron diversas técnicas de regularización:

*   **Regularización L1/L2:** En el modelo de Regresión Logística, se exploraron los parámetros de regularización `penalty` y `C` para controlar la complejidad del modelo.
*   **Parámetros de los árboles:** En los modelos basados en árboles como Random Forest y Gradient Boosting, hiperparámetros como `max_depth`, `min_samples_leaf` y `n_estimators` actúan como regularizadores al limitar la complejidad de los árboles de decisión individuales.
*   **Dropout:** En el Perceptrón Multicapa (MLP), se incluyeron capas de `Dropout` en la arquitectura. Esta técnica desactiva aleatoriamente un porcentaje de neuronas durante el entrenamiento, forzando a la red a aprender representaciones más robustas y menos dependientes de neuronas específicas.

Los resultados de los modelos optimizados, como se puede apreciar en las matrices de confusión (`confusion_matrix_hyperparams.png`) y curvas ROC (`roc_curves_hyperparams.png`) del MLP optimizado, reflejan el efecto de este proceso de ajuste y regularización.

