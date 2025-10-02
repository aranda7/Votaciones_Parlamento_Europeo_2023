# Memoria Académica: Hito 5 - Interpretación y Evaluación de Sesgos

## Introducción

Esta memoria se enfoca en la interpretación de los modelos de machine learning y la evaluación de posibles sesgos. La confianza y la transparencia son esenciales en la implementación de sistemas de inteligencia artificial, y este hito aborda directamente estos aspectos. El objetivo es desmitificar la "caja negra" de los modelos, entender qué características impulsan sus decisiones y analizar si estas decisiones afectan de manera desproporcionada a los diferentes subgrupos de la población. Para ello, se utiliza la técnica SHAP (SHapley Additive exPlanations) como herramienta principal, aplicada a los cuatro modelos optimizados en las fases anteriores.



## 5.1. Interpretación del Modelo con SHAP

Este punto se abordó utilizando exclusivamente la metodología **SHAP (SHapley Additive exPlanations)**. La elección se fundamenta en su sólida base teórica, derivada de la teoría de juegos cooperativos, que permite asignar a cada característica una contribución específica y justa a la predicción final. A diferencia de otras técnicas locales como LIME, SHAP garantiza consistencia y exactitud, asegurando que la suma de las importancias de las características sea igual a la predicción del modelo.

### 5.1.1. Cálculo de los Valores SHAP

Se calcularon los valores SHAP para los cuatro modelos sobre un subconjunto del conjunto de datos de prueba para mantener la eficiencia computacional. Se utilizaron los `explainers` apropiados para cada tipo de modelo:

*   **`shap.TreeExplainer`** para los modelos basados en árboles (Random Forest y Gradient Boosting).
*   **`shap.KernelExplainer`** para el modelo de Regresión Logística.
*   **`shap.DeepExplainer`** para el modelo de Perceptrón Multicapa (MLP) desarrollado en PyTorch.

Los resultados de este análisis, que incluyen los `summary plots` y los `dependence plots`, se encuentran detallados en el notebook `hito5.ipynb`.

### 5.1.2. Análisis de la Importancia de las Características

El análisis de los valores SHAP permitió identificar las características más influyentes para cada modelo. Los `summary plots` generados para cada algoritmo muestran la distribución de los valores SHAP para cada característica, revelando no solo qué variables son más importantes, sino también cómo afectan a la predicción (un valor SHAP positivo indica un aumento en la probabilidad de la clase predicha, y viceversa).

Se observó una consistencia notable entre los modelos en cuanto a las características más relevantes. Variables como la **intención de voto previa (`intencion_voto_encoded`)** y algunas de las componentes principales derivadas del análisis de PCA (`categorico_pca_0`, `categorico_pca_1`, etc.) aparecieron consistentemente en los primeros puestos de importancia, lo que valida su relevancia en el problema de predicción.



## 5.2. Evaluación de Sesgos

Una de las aplicaciones más importantes de la interpretabilidad es la capacidad de auditar los modelos en busca de sesgos. Un modelo se considera sesgado si su rendimiento o su comportamiento varía significativamente entre diferentes subgrupos de la población, especialmente aquellos definidos por características sensibles.

### 5.2.1. Metodología de Análisis de Sesgos

El análisis de sesgos se centró en dos variables sensibles identificadas en el conjunto de datos:

*   **Género (`genero_encoded`)**
*   **Autoubicación Ideológica (`autoubicacion_ideologica_encoded`)**

La estrategia consistió en segmentar los datos según los valores de estas variables y analizar si existían disparidades en el comportamiento del modelo. Para ello, se comparó la distribución de los valores SHAP para cada característica entre los diferentes subgrupos. Una diferencia sistemática en la magnitud o dirección de los valores SHAP entre, por ejemplo, hombres y mujeres, podría ser un indicador de sesgo.

### 5.2.2. Hallazgos del Análisis de Sesgos

El análisis, documentado en el notebook `hito5.ipynb`, reveló los siguientes puntos:

*   **Diferencias de Escala:** Se observó que la escala de los valores SHAP variaba considerablemente entre los diferentes tipos de modelos, lo que impidió una comparación numérica directa de las magnitudes de SHAP entre ellos. Por ejemplo, los valores para la Regresión Logística fueron de un orden de magnitud mucho mayor que para los modelos de árboles.

*   **Indicios de Disparidad:** A pesar de las diferencias de escala, se encontraron patrones consistentes. Tanto en el modelo Random Forest como en el MLP, se observaron indicios de que ciertos grupos ideológicos recibían, en promedio, contribuciones SHAP de mayor magnitud. Las diferencias por género, aunque presentes, fueron menos pronunciadas.

*   **Conclusión Provisional:** El análisis sugiere la existencia de disparidades en el tratamiento de los subgrupos por parte de los modelos, especialmente en relación con la autoubicación ideológica. Sin embargo, como se señala en las conclusiones del notebook, para confirmar un **sesgo sistemático** se requeriría un análisis más profundo, incluyendo pruebas estadísticas y una visualización más detallada de las diferencias por característica.

### 5.2.3. Recomendaciones y Pasos Futuros

El análisis concluyó con una serie de recomendaciones para futuros trabajos, entre las que se incluyen:

*   Mejorar la organización y visualización de los resultados de SHAP para facilitar la comparación entre subgrupos.
*   Realizar un análisis estadístico para confirmar la significancia de las disparidades observadas.
*   En caso de confirmarse la existencia de sesgos, aplicar técnicas de mitigación, como el reequilibrio de datos, el ajuste de umbrales de decisión o el uso de algoritmos de aprendizaje justo (*fairness-aware learning*).

