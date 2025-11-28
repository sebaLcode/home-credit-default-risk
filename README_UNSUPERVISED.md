# Reporte de Aprendizaje No Supervisado: Segmentación de Clientes

## 1. Contexto y los datos utilizados
Se desea desarrollar un modelo de scoring crediticio mediante técnicas de aprendizaje no supervisado, siguiendo la lógica de CRISP-DM, los datos utilizados son del dataset Home Credit Default Risk (https://www.kaggle.com/competitions/home-credit-default-risk/overview), para ello fueron entregados los datos en formato .parquet, en donde se trabajó con el dataset principal en busca de un **Análisis de clusters sobre los clientes**.
* **Archivo:** `application_.parquet`
* Obteniendo una dimensión de **307.511 filas** y **122 columnas**.

De el conjunto total, fueron seleccionadas **12 variables claves** que representan las dimensiones financieras, demográficas y de comportamento, además la variable objetivo (`TARGET`) u otras como el id, no fueron seleccionadas para no sesgar el agrupamiento. Entonces las variables utilizadas fueron:

* 'AMT_INCOME_TOTAL'
* 'AMT_CREDIT'
* 'AMT_ANNUITY'
* 'AMT_GOODS_PRICE'
* 'DAYS_BIRTH'
* 'DAYS_EMPLOYED'
* 'CNT_CHILDREN'
* 'CNT_FAM_MEMBERS'
* 'OWN_CAR_AGE'
* 'EXT_SOURCE_1'
* 'EXT_SOURCE_2'
* 'EXT_SOURCE_3'

## 2. Selección del Algoritmo
De los algoritmos disponibles sugeridos para el EA3 (Clustering, Detección de Anomalías, Reducción de la dimensionalidad, etc), se seleccionó:

* **Análisis de Clusters sobre los clientes con K-Means**, dada la eficiencia computacional con grandes volúmenes de datos y su capacidad para generar segmentos compactos. Además, mediante el método del codo, se determinó el número optimo de segmentos, llegando a un valor de **K=3**


## 3. Metodología y preprocesamiento
Para preparar los datos, se aplicó el siguiente flujo:
1.  **Selección de Características:** Se redujo la dimensionalidad a una matriz de **(307511, 12)**.
2.  **Imputación:** Se rellenaron los valores nulos utilizando la mediana para mantener la robustez ante outliers.
3.  **Escalado:** Se aplicó `StandardScaler` para normalizar todas las variables.

## 4. Resultados obtenidos
El algoritmo logró separar la población en 3 grupos con perfiles de riesgo y comportamiento muy diferenciados. A continuación, se interpretará los centroides obtenidos:

### Resumen de Riesgo por Cluster
Se cruzaron los clusters resultantes con la variable `TARGET` (tasa de morosidad) para validar la utilidad del agrupamiento:

| Cluster | Cantidad de Clientes | Tasa de Incumplimiento (Riesgo) | Perfil Identificado |
| :--- | :--- | :--- | :--- |
| **1** | 72.691 | **9,8% (Alto)** | **Jóvenes Vulnerables:** Grupo más joven (36 años aprox.) y con mayor cantidad de hijos promedio (1.47), que poseen la tasa de mora más alta. |
| **2** | 161.664 | **8,4% (Medio)** | **Masivo Bajos Ingresos:** El grupo más grande. Tienen los ingresos ($144.000 aprox.) y montos de crédito más bajos. |
| **0** | 73.156 | **5,7% (Bajo)** | **Clientes Consolidados:** Grupo de edad parecida al cluster 2 pero con estabilidad, con los ingresos más altos ($230.000 aprox.) y solicitan créditos altos ($1.140.000 aprox.).|


Podemos notar que existe una diferencia de casi el doble entre el riesgo del Cluster 0 y el Cluster 1, también se puede decir que el grupo del Cluster 0 es el más seguro.

## 5. Conclusión
Tras analizar los resultados del agrupamiento K-means y validarlos con la tasa de morosidad, se confirmó que los Clusters creados separan de buena manera a los clientes riesgosos de los seguros, por lo que seguiremos ocupando el modelo de K-Means para el desarrollo del exámen.