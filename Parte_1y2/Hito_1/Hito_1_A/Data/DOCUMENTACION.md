# Documentación de Preprocesamiento de Datos

Este documento detalla las transformaciones y codificaciones aplicadas a las columnas del dataset original para preparar los datos para el modelado.

## Columnas Procesadas:

### `ingreso_hogar`
* **Transformación:** Codificación ordinal de los habitantes por municipio.
* **Mapeo Original -> Codificado:**
    * `'N.S.'`  -> `0` 
    * `'Menos de 1.100' &euro;'`  -> `1` 
    * `'De 1.100 a 1.800 &euro;'` -> `2`
    * `'De 1.801 a 2.700 &euro;'` -> `3`
    * `'De 2.701 a 3.900 &euro;'` -> `4`
    * `'De 3.901 a 5.000 &euro;'` -> `5`
    * `'Más de 5.000 &euro;'`     -> `6` 

### `Habitantes_municipio`
* **Transformación:** Codificación ordinal de los habitantes por municipio.
* **Mapeo Original -> Codificado:**
    * `'N.S.'`  -> `0` 
    * `'Menos o igual a 2.000 habitantes'`  -> `1` 
    * `'2.001 a 10.000 habitantes'`         -> `2` 
    * `'10.001 a 50.000 habitantes'`        -> `3` 
    * `'50.001 a 100.000 habitantes'`       -> `4` 
    * `'100.001 a 400.000 habitantes'`      -> `5` 
    * `'400.001 a 1.000.000 habitantes'`    -> `6` 
    * `'Más de 1.000.000 habitantes'`       -> `7` 

### `Intención_voto_encoded` (variable objetivo) 
* **Transformación:** Codificación ordinal de la intención de voto general.
* **Mapeo Original -> Codificado:**
    * `'No sabe todavía'`              -> `0` 
    * `'N-C'`                          -> `0`
    * `'En Blanco'`                    -> `0`
    * `'Voto nulo'`                    -> `0`
    * `'No votaría'`                   -> `0`
    * `'Adelnate Andalucía'`           -> `1`
    * `'Andalucía Por Sí'`             -> `2`
    * `'No BNG'`                       -> `3`
    * `'CCa'`                          -> `4`
    * `'CUP'`                          -> `5`
    * `'Caminando Juntos'`             -> `6`
    * `'Ciudadanos'`                   -> `7`
    * `'Compromís'`                    -> `8`
    * `'EAJ-PNV'`                      -> `9`
    * `'EH Bildu'`                     -> `10`
    * `'ERC'`                          -> `11`
    * `'En Comú Podem'`                -> `12`
    * `'Escaños en Blanco'`            -> `13`
    * `'España Vaciada'`               -> `14`
    * `'Falange Española de las Jons'` -> `15`
    * `'Frente Obrero'`                -> `16`
    * `'IU'`                           -> `17`
    * `'Jaén Merece Más'`              -> `18`
    * `'JxCat'`                        -> `19`
    * `'Los Verdes'`                   -> `20`
    * `'Nueva Canarias'`               -> `21`
    * `'Otro Partido'`                 -> `22`
    * `'Pacma'`                        -> `23`
    * `'PAR'`                          -> `24`
    * `'PP'`                           -> `25`
    * `'PSOE'`                         -> `26`
    * `'Partido Libertario'`           -> `27`
    * `'Podemos'`                      -> `28`
    * `'Sumar'`                        -> `29`
    * `'Teruel Existe'`                -> `30`
    * `'VOX'`                          -> `31`


* **mapa_bloques_ideologicos =** 
    #### No voto
    `'No sabe todavía': 'No voto'`,
    `'N.C.': 'No voto'`,
    `'No votaría': 'No voto'`,
    `'En blanco': 'No voto'`,
    `'Voto nulo': 'No voto'`,

    #### Izquierda
    `'PSOE': 'Izquierda'`,
    `'Sumar': 'Izquierda'`,
    `'Podemos': 'Izquierda'`,
    `'IU': 'Izquierda'`,
    `'En Comú Podem': 'Izquierda'`,
    `'Frente Obrero': 'Izquierda'`,
    `'EH Bildu': 'Izquierda'`,

    #### Centro/Izquierda (Regionalistas progresistas)
    `'ERC': 'Centro/Izquierda'`,
    `'BNG': 'Centro/Izquierda'`,
    `'Compromís': 'Centro/Izquierda'`,
    `'Adelante Andalucía': 'Centro/Izquierda'`,
    `'Andalucía Por Sí': 'Centro/Izquierda'`,

    #### Centro / Liberal
    `'Ciudadanos': 'Centro/Liberal'`,
    `'Partido Libertario': 'Centro/Liberal'`,
    `'Los Verdes': 'Centro/Liberal'`,
    `'EAJ-PNV': 'Centro/Liberal'`,

    #### Centro/Derecha
    `'PP': 'Centro/Derecha'`,
    `'PAR': 'Centro/Derecha'`,
    `'CCa': 'Centro/Derecha'`,
    `'Nueva Canarias': 'Centro/Derecha'`,
    `'JxCat': 'Centro/Derecha'`,

    #### Extrema Derecha
    `'VOX': 'Extrema Derecha'`,
    `'Falange Española de las JONS'`: `'Extrema Derecha'`,
    `'Caminando Juntos'`: `'Extrema Derecha'`,
    `'PACMA`: `'Extrema Derecha'`,  # Aunque PACMA es un partido animalista, en el contexto de la clasificación política se ha incluido aquí por su enfoque en temas específicos.

    #### Localistas / Minoritarios
    `'España Vaciada': 'Localistas/Minoritarios'`,
    `'Teruel Existe': 'Localistas/Minoritarios'`,
    `'Jaén Merece Más': 'Localistas/Minoritarios'`,
    `'Otro partido': 'Localistas/Minoritarios'`,
    `'Escaños en Blanco': 'Localistas/Minoritarios'`,
    `'CUP': 'Localistas/Minoritarios'`,


### `probabilidad_voto_generales`
* **Transformación:** Limpieza de texto, conversión a numérico y normalización a escala [0, 1].
* **Detalles:** Valores 'N.S.', 'N.C.' tratados como nulos y luego imputados. Frases como '10 Con toda seguridad, iría a votar' convertidas a su valor numérico. La escala original de 1-10 se dividió por 10.

### `Renta_Per_Capita_2023_miles_euros_escalada`
* **Transformación:** Estandarización utilizando `StandardScaler`.
* **Detalles:** La columna original `Renta_Per_Capita_2023_miles_euros` fue transformada para tener media 0 y desviación estándar 1. La columna original fue eliminada.

### `provincia_pca_0` a `provincia_pca_9` (Componentes PCA de 'provincia')
* **Transformación:** Reducción de dimensionalidad mediante Análisis de Componentes Principales (PCA).
* **Detalles:** Se aplicó PCA a las variables dummy (one-hot encoded) de la columna 'provincia'. Se retuvieron 10 componentes principales para capturar la mayor varianza posible. 

### `genero_encoded`
* **Transformación:** Codificación numérica (Label Encoding).
* **Mapeo Original -> Codificado:**
    * `'Hombre'` -> `0` 
    * `'Mujer'`  -> `1` 

### `percepcion_clase_encoded`
* **Transformación:** Codificación numérica (Ordinal Encoding).
* **Mapeo Original -> Codificado:** 
    * `'Clase alta'`              -> `'Alta'`      -> `2`
    * `'Clase media-alta'`        -> `'Media'`     -> `1`
    * `'Clase media-media'`       -> `'Media'`     -> `1`
    * `'Clase media-baja'`        -> `'Media'`     -> `1`
    * `'Clase baja'`              -> `'Baja'`      -> `1`
    * `'Clase trabajadora/obrera'`-> `'Baja'`      -> `1`
    * `'Clase pobre'`             -> `'Baja'`      -> `0`
    * `'Proletariado'`            -> `'Baja'`      -> `0`
    * `'A los/as de abajo'`       -> `'Baja'`      -> `0`
    * `'Excluidos/as'`            -> `'Baja'`      -> `0`
    * `'A la gente común'`        -> `'Baja'`      -> `0`    # Incluida en 'Baja'
    . Categorías que irán a NaN y luego se eliminarán
    * `'No sabe, duda'`           -> `pd.NA`
    * `'N.C.'`                    -> `pd.NA`
    * `'No cree en las clases'`.  -> `pd.NA`,
    * `'Otras'`                   -> `pd.NA`
    * `'nan'`                     -> `pd.NA` # Para los NaNs que pudieran haberse convertido a string 'nan'

### `nivel_educacion_encoded`
* **Transformación:** Codificación numérica (Ordinal Encoding).
* **Mapeo Original -> Codificado:** (Listar aquí el mapeo exacto de niveles de educación)
    * `'Educación Básica'`             -> `1` 
    * `'Bachillerato/FP Media'`        -> `2`
    * `'FP Superior'`                  -> `3`
    * `'Grado/Diplomatura'`            -> `4`
    * `'Postgrado/Universitario Alto'` -> `5`

### `autoubicacion_ideologica_encoded`
* **Transformación:** Doble proceso:
    1.  Limpieza y conversión de la escala 1-10 (incluyendo manejo de 'N.S.', 'N.C.' y frases).
    2.  Agrupación en categorías y posterior codificación ordinal.
* **Mapeo de Categorías -> Codificado:**
    * `'izquierda'` (originalmente 1-4) -> `1`
    * `'centro'` (originalmente 5-6)    -> `2`
    * `'derecha'` (originalmente 7-10)  -> `3`

### `valoracion_economia_sin_UE_encoded`
* **Mapeo de Categorías -> Codificado Ordinal:**
    * `'Peor'`        -> `0` 
    * `'Igual'`       -> `1` 
    * '`Mejor'`       -> `2`