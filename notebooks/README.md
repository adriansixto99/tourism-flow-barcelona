# Guía de notebooks

Los notebooks contienen el código experimental del TFM. Todos están orientados a la predicción mensual de `Pernoctaciones` en Barcelona con un horizonte de un mes y una ventana final de test de 18 meses.

## Preparación de datos

`data_etl.ipynb` es el cuaderno de extracción, integración y preprocesamiento previo a los algoritmos. Incluye las etapas de:

- extracción de `Pernoctaciones` desde el archivo `2074.csv` del INE y limpieza de fechas y separadores de miles;
- exploración de la serie histórica;
- preparación de AEMET, calendario de Catalunya, AENA y Google Trends;
- integración mensual mediante un `INNER JOIN`;
- ingeniería de características, incluyendo retardos, medias móviles, codificación cíclica del mes e indicador COVID-19;
- división cronológica train/test y matriz de correlación;
- bloques de referencia de SARIMA, Regresión Lineal, Random Forest, XGBoost y LSTM.

El dataset generado por esta fase se denomina `DATASET_TFM_AVANZADO.csv` en el flujo de Colab. Los archivos de entrada y los productos intermedios no se incluyen; su trazabilidad y nombres esperados están documentados en `../data/README.md`.

## Flujo común

1. Montaje de Google Drive en Colab.
2. Instalación de las dependencias específicas del modelo.
3. Lectura y ordenación cronológica del dataset.
4. Entrenamiento utilizando únicamente información disponible antes del mes objetivo.
5. Predicción y evaluación sobre valores absolutos de pernoctaciones.
6. Conversión de la predicción a nivel IPT mediante los terciles históricos P33/P66.
7. Exportación de métricas, alertas y matriz de confusión.

## Notas específicas por notebook

### `data_etl.ipynb`

Cuaderno de preparación end-to-end. Sirve como punto de entrada para reproducir la construcción de la matriz de modelado antes de ejecutar los notebooks de cada algoritmo. Sus bloques de modelado pertenecen a una ejecución de referencia.

### `sarima_code.ipynb`

Modelo estadístico univariante. Modela directamente la serie de pernoctaciones, utiliza estacionalidad anual (`m=12`) y selecciona órdenes mediante `auto_arima` y una búsqueda de afinado con `SARIMAX`. Evalúa cada mes mediante walk-forward one-step-ahead incorporando el valor real observado después de cada predicción.

### `lr_code.ipynb`

Modelo lineal sobre la diferencia mensual entre el valor objetivo y el valor de referencia. Compara OLS con Ridge mediante validación temporal y conserva coeficientes para facilitar la interpretación de las variables.

### `rf_code.ipynb`

Modelo de Random Forest sobre variables de calendario, retardos, medias móviles, dispersión y señales exógenas retardadas. Utiliza `RandomizedSearchCV` seguido de `GridSearchCV`, ambos con `TimeSeriesSplit`.

### `xgboost_code.ipynb`

Modelo de boosting sobre el mismo objetivo diferenciado y el mismo principio de features seguras que Random Forest. El informe lo selecciona como referencia operativa del IPT por alcanzar 94.4% de acierto de nivel de alerta, aunque SARIMA logra menor MAPE.

### `lstm_code.ipynb`

Modelo recurrente que recibe una ventana de 12 meses de diferencias escaladas y variables de calendario del mes objetivo. La normalización se ajusta sobre train. Compara arquitecturas candidatas y aplica early stopping y reducción de learning rate.