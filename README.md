# Comparación de Modelos Supervisados usando Validación Cruzada
### Por: Samuel Angel Cardona
## Objetivo
El propósito de este trabajo práctico es comparar el rendimiento de distintos modelos de regresión supervisada para predecir el precio de venta de viviendas, aplicando la técnica de validación cruzada ($K\text{-Fold}$) como herramienta principal de evaluación. El ejercicio busca desarrollar una comprensión crítica sobre la selección de modelos y su capacidad de generalización.

## Datos
El conjunto de datos empleado corresponde a un dataset público de kaggle, del mercado inmobiliario (`enhanced_house_price_dataset.csv`), sin valores nulos ni duplicados.
https://www.kaggle.com/datasets/chershi/house-price-prediction-dataset-2000-rows 

* **Variable objetivo:** `Price` (precio de venta del inmueble en USD).
* **Tipo de problema:** Regresión supervisada.
* **Observaciones:** 2.000 viviendas.
* **Variables predictoras numéricas (7):** `Area`, `Bedrooms`, `Bathrooms`, `Stories`, `Parking`, `Age`, `Locality Rating`.
* **Variables predictoras categóricas (8):** `City`, `Furnishing`, `Main Road`, `Guest Room`, `Basement`, `Water Supply`, `Air Conditioning`, `Preferred Tenant`.

## Metodología y Preprocesamiento
* **Partición de datos:** División en tres subconjuntos disjuntos: 70% entrenamiento ($1.400$), 15% validación ($300$) y 15% prueba ($300$) con `random_state=42`.
* **Pipeline:** Estandarización con `StandardScaler` para cuantitativas y codificación `OneHotEncoder(drop='first')` para cualitativas mediante `ColumnTransformer` para prevenir *data leakage*.
* **Validación cruzada:** Esquema $5\text{-Fold}$ aplicado sobre el conjunto de entrenamiento usando como métrica principal el $\text{RMSE}$.

## Modelos Evaluados
* **Linear Regression:** Modelo base lineal por mínimos cuadrados ordinarios.
* **Ridge:** Regresión lineal con regularización $L_2$.
* **Lasso:** Regresión lineal con regularización $L_1$.
* **KNN (K-Nearest Neighbors):** Estimador no paramétrico basado en distancias euclidianas ($k=5$).
* **Árbol de Decisión:** Modelo no paramétrico de partición recursiva sin poda.
* **Random Forest:** Ensamble de 100 árboles de decisión por promediación (*bagging*).

## Tabla Comparativa de Resultados

| Modelo | Train RMSE (USD) | Val RMSE (USD) | CV RMSE Promedio (USD) | CV RMSE Desv. Est. (USD) |
| :--- | :---: | :---: | :---: | :---: |
| **Ridge** | $151.665,41$ | $147.078,07$ | $153.631,87$ | $8.515,90$ |
| **Lasso** | $151.664,63$ | $147.161,02$ | $153.649,71$ | $8.544,41$ |
| **Linear Regression** | $151.664,63$ | $147.163,69$ | $153.650,87$ | $8.544,54$ |
| **Random Forest** | $68.320,15$ | $161.840,90$ | $168.450,22$ | $9.120,30$ |
| **KNN** | $168.337,27$ | $195.135,67$ | $207.058,28$ | $8.061,25$ |
| **Árbol de Decisión** | $0,00$ | $258.748,76$ | $279.887,98$ | $12.878,55$ |

## Evaluación Final en Prueba (Ridge)
* **RMSE en Validación Cruzada (Promedio):** $153.631,87\text{ USD}$
* **RMSE en Prueba (Test):** $145.802,31\text{ USD}$
* **MAE en Prueba (Test):** $117.322,33\text{ USD}$
* **Coeficiente de Determinación ($R^2$):** $0,7466$

## Conclusiones
* **Ridge** fue el modelo más eficiente y estable, logrando el menor error promedio en validación cruzada y la menor dispersión ($\pm 8.515,90\text{ USD}$), explicando el $74,7\%$ de la variabilidad del precio en datos no vistos ($R^2 = 0,7466$).
* **Árbol de Decisión** memorizó completamente los datos de entrenamiento ($\text{RMSE} = 0$), pero sufrió un sobreajuste severo en validación cruzada ($\text{RMSE} = 279.887,98$), evidenciando la necesidad de restringir su profundidad o usar ensambles.
* **Random Forest** redujo la varianza del árbol individual, pero los modelos lineales regularizados mantuvieron un mejor desempeño debido a la naturaleza aditiva de los predictores.
* **Validación Cruzada:** Permitió evaluar la estabilidad de los estimadores, demostrando que el error estimado en entrenamiento ($153.631,87\text{ USD}$) aproxima de forma precisa el error en la prueba final ($145.802,31\text{ USD}$).
