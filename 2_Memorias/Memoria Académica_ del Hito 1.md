# Memoria Académica: Análisis y Evaluación del Hito 1

## Introducción

El presente documento constituye una memoria académica que analiza y evalúa el trabajo realizado en el marco del Hito 1 de la práctica final del Máster en Inteligencia Artificial. El objetivo principal del proyecto es el desarrollo de un modelo de inteligencia artificial capaz de predecir la intención de voto en las elecciones al Parlamento Europeo de 2024, a partir de un conjunto de datos sociodemográficos y de opinión extraídos del estudio Nº 3452 del Centro de Investigaciones Sociológicas (CIS).

Esta memoria se centra en la evaluación de las dos tareas fundamentales del Hito 1: la **selección y preparación del modelo** y el **preprocesamiento de datos**. Se analizará la idoneidad de las decisiones tomadas, la rigurosidad de los procedimientos aplicados y el cumplimiento de las consignas académicas establecidas, todo ello en un tono ftécnico y análitico.



## 1. Preprocesamiento y Preparación de los Datos

La fase de preprocesamiento de datos es un pilar fundamental en cualquier proyecto de aprendizaje automático, ya que de la calidad y adecuación de los datos de entrada depende en gran medida el rendimiento del modelo. En este proyecto, se ha llevado a cabo un exhaustivo proceso de transformación y limpieza del dataset original, con el objetivo de prepararlo para los algoritmos de clasificación seleccionados. A continuación, se detallan las principales técnicas de preprocesamiento aplicadas, justificando su relevancia en el contexto del problema.

### 1.1. Codificación de Variables Categóricas

Una parte significativa de las variables del dataset original son de naturaleza categórica. Para que los modelos de machine learning puedan procesar esta información, ha sido necesario convertirlas a un formato numérico. Se han empleado dos estrategias principales:

*   **Codificación Ordinal:** Para variables con una relación de orden intrínseca, como `ingreso_hogar`, `Habitantes_municipio` o `nivel_educacion_encoded`, se ha utilizado una codificación ordinal. Esta técnica asigna un número entero a cada categoría, preservando la jerarquía entre ellas. Por ejemplo, en la variable `ingreso_hogar`, se asigna un valor de `1` a 'Menos de 1.100 €' y un valor de `6` a 'Más de 5.000 €', reflejando el orden creciente de los niveles de ingreso.

*   **Codificación One-Hot y Reducción de Dimensionalidad con PCA:** Para variables categóricas nominales sin un orden inherente, como `provincia`, la codificación ordinal no es apropiada. En su lugar, se ha optado por una estrategia más sofisticada. Primero, se aplicaría una codificación *one-hot* para crear variables *dummy* para cada provincia. Sin embargo, para evitar la "maldición de la dimensionalidad" que esto generaría, se ha aplicado un Análisis de Componentes Principales (PCA) sobre estas variables *dummy*. De esta forma, se ha reducido la dimensionalidad a 10 componentes principales (`provincia_pca_0` a `provincia_pca_9`), que captan la mayor parte de la varianza de la variable original en un espacio de características mucho más reducido.

### 1.2. Transformación de Variables Numéricas

Las variables numéricas también han sido objeto de transformaciones para optimizar su uso en los modelos:

*   **Normalización y Estandarización:** La variable `probabilidad_voto_generales` se ha normalizado a una escala de 0 a 1, mientras que `Renta_Per_Capita_2023_miles_euros` ha sido estandarizada (media 0 y desviación estándar 1) utilizando `StandardScaler`. Estas transformaciones son determinantes para algoritmos sensibles a la escala de las características, como la Regresión Logística o el Perceptrón Multicapa, asegurando que todas las variables contribuyan de manera equitativa al aprendizaje del modelo.

### 1.3. Tratamiento de la Variable Objetivo

La variable objetivo, `Intención_voto_encoded`, ha sido cuidadosamente procesada. Se ha realizado una codificación ordinal, agrupando las respuestas de 'No sabe', 'No contesta', 'En Blanco' y 'Voto nulo' en una única categoría (`0`), y asignando un identificador numérico único a cada partido político. Además, se ha creado una agrupación por bloques ideológicos (`mapa_bloques_ideologicos`), lo que podría permitir en futuras fases del proyecto un análisis a un nivel más agregado.

En resumen, el preprocesamiento de datos ha sido estudiado desde un enfoque meticuloso y bien justificado. Se han seleccionado técnicas apropiadas para cada tipo de variable, abordando problemas comunes como la alta dimensionalidad y la escala de las características. Esta preparación sienta una base sólida para la siguiente fase del proyecto: la selección y el entrenamiento de los modelos de clasificación.



## 2. Selección y Justificación de Modelos

La elección de los algoritmos de aprendizaje automático es una decisión crítica que define la capacidad del proyecto para resolver el problema de clasificación planteado. En el Hito 1, se ha propuesto una selección de cuatro modelos que abarcan diferentes paradigmas del aprendizaje automático, permitiendo un análisis comparativo robusto y una evaluación exhaustiva del rendimiento. La justificación académica para la selección de estos modelos se basa en una estrategia metodológica que busca un equilibrio entre interpretabilidad, rendimiento y complejidad.

### 2.1. Modelos Seleccionados

El notebook `Hito_1B.ipynb` presenta una justificación detallada para la elección de los siguientes modelos:

1.  **Regresión Logística (Logistic Regression):** Este modelo se ha seleccionado como una **línea base (baseline)** fundamental. Su simplicidad y alta interpretabilidad permiten establecer un punto de referencia de rendimiento. La Regresión Logística es un modelo lineal que, a pesar de su sencillez, es muy eficaz para problemas de clasificación binaria y multiclase. Su inclusión es funcional desde el punto de vista de la metodología ya que evalúa si los modelos más complejos aportarán o no una mejora significativa que justifique su mayor coste computacional y menor interpretabilidad.

2.  **Random Forest Classifier (Bosque Aleatorio):** Como representante de los métodos de ensamble basados en *bagging*, el Random Forest se ha elegido por su **robustez y su capacidad para manejar relaciones no lineales**. Este modelo construye múltiples árboles de decisión durante el entrenamiento y combina sus predicciones para obtener un resultado más preciso y estable. Es menos propenso al sobreajuste que un único árbol de decisión y proporciona una medida de la importancia de las características, lo que añade una capa de interpretabilidad al análisis.

3.  **Gradient Boosting:** Este es otro método de ensamble, pero a diferencia de Random Forest, se basa en la técnica de *boosting*. Los modelos de Gradient Boosting construyen los árboles de forma secuencial, donde cada nuevo árbol corrige los errores del anterior. Se ha seleccionado por su **alto rendimiento predictivo**, siendo a menudo uno de los algoritmos más competitivos en problemas de clasificación con datos tabulares. Su inclusión permite explorar el límite superior del rendimiento alcanzable con los datos disponibles.

4.  **Perceptrón Multicapa (MLP) / Red Neuronal:** La inclusión de un MLP introduce el **paradigma del aprendizaje profundo (Deep Learning)** en el proyecto. Aunque más complejo de entrenar y optimizar, un MLP tiene la capacidad de aprender patrones muy complejos y no lineales en los datos. Su selección se justifica por la necesidad de explorar si una arquitectura de red neuronal podría captar relaciones sutiles que los modelos más tradicionales  dejaríanpasar por alto, ofreciendo potencialmente un rendimiento superior.

### 2.2. Justificación de la Selección

La selección de estos cuatro modelos es académicamente sólida y metodológicamente coherente. Se ha seguido un enfoque en el que se incrementa la complejidad: comenzando con un modelo lineal simple (Regresión Logística), pasando por modelos de ensamble potentes y robustos (Random Forest y Gradient Boosting), y culminando con un modelo de aprendizaje profundo (MLP). Esta estrategia no solo permite la comparación rigurosa del rendimiento, sino que también facilita la comprensión más profunda de la naturaleza del problema y de las características del dataset.

La justificación proporcionada en el notebook `Hito_1B.ipynb` busca ser exhaustiva y demostrar una comprensión clara de las fortalezas y debilidades de cada modelo, así como de su contribución específica al proyecto. Esta selección estratégica sienta las bases para las fases posteriores de entrenamiento, optimización y evaluación de modelos.



## 3. Consignas del Hito 1

Una vez analizados el preprocesamiento de los datos y la selección de modelos, es fundamental evaluar en qué medida el trabajo realizado se ajusta a las consignas específicas del Hito 1, tal y como se describen en el documento de la práctica. El Hito 1 se divide en dos componentes principales: la **selección del modelo** (1 punto) y el **preprocesamiento de datos**.

### 3.1. Selección del Modelo 

La consigna para este apartado es: "*Elegir los modelos de machine learning o deep learning que se entrenarán. Justificar la elección en base al tipo de problema (clasificación, regresión, etc.), las características del dataset y el rendimiento esperado de cada algoritmo*."

**Evaluación:**

Se justifica la selección de los cuatro modelos (Regresión Logística, Random Forest, Gradient Boosting y MLP) de manera académicamente rigurosa, detallada y bien fundamentada. Para cada modelo, se explica claramente que:

*   **El tipo de problema:** Se identifica correctamente como un problema de clasificación.
*   **Las características del dataset:** Se consideran implícitamente al justificar la necesidad de modelos que puedan manejar la no linealidad y la complejidad de las interacciones entre variables sociodemográficas.
*   **El rendimiento esperado:** Se establece una expectativa de rendimiento para cada modelo, desde la línea base de la Regresión Logística hasta el potencial de alto rendimiento de Gradient Boosting y MLP.
*   **La contribución metodológica:** Se va más allá de la simple elección de modelos y se explica el rol estratégico de cada uno dentro del proyecto (línea base, robustez, rendimiento, exploración de paradigmas).

A través de la justificación se intenta mostrar una comprensión profunda de la teoría subyacente a cada algoritmo y de su aplicación práctica. 

### 3.2. Preprocesamiento de Datos 

La consigna para este apartado es: "*Realizar el preprocesamiento adecuado para cada modelo, asegurando que los datos están en un formato compatible y optimizado para los algoritmos seleccionados (normalización, estandarización, codificación, etc.)*."

**Evaluación:**

Se enfrenta preprocesamiento de datos, documentado en `DOCUMENTACION.md` y ejecutado en el notebook `Hito_1A.ipynb`, desde un punto de vista **exhaustivo**. Se han aplicado las técnicas que se consideran correctas para cada tipo de variable, teniendo en cuenta la preparación de datos para el modelado.

*   **Codificación:** Se ha utilizado codificación ordinal y una combinación de one-hot con PCA para manejar las variables categóricas.
*   **Normalización y Estandarización:** Se han aplicado transformaciones de escala a las variables numéricas, lo cual es esencial para el correcto funcionamiento de varios de los modelos seleccionados.
*   **Limpieza:** Se han tratado los valores nulos y se han limpiado las variables para asegurar la calidad de los datos.

El resultado es un conjunto de datos (`data_ML.csv`) estructurado y optimizado para ser utilizado por los modelos de machine learning. El trabajo realizado en esta área apunta con diligencia a la necesidad de  asegurar una base de datos de alta calidad. 



## 4. Conclusiones

El preprocesamiento de los datos ha sido meticuloso, aplicando técnicas adecuadas para transformar un dataset complejo en un formato optimizado para el modelado. La selección de modelos está justificada, proponiendo un abanico de algoritmos que permitirá una evaluación comparativa exhaustiva en las fases posteriores del proyecto.


