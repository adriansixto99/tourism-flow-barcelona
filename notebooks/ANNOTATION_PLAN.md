# Plan de anotación

## Alcance

El cuaderno público es una copia anotada del código original. Las celdas
Markdown explican el objetivo, el significado de negocio, las entradas, las
salidas y las limitaciones de cada etapa.

El cuaderno sigue esta narrativa:

1. Definir y limpiar la variable objetivo de pernoctaciones.
2. Inspeccionar la serie mensual histórica.
3. Preparar los datos meteorológicos, de calendario, aeropuerto e interés de
   búsqueda.
4. Unir las fuentes mensuales.
5. Crear variables temporales y contextuales.
6. Dividir las observaciones cronológicamente.
7. Inspeccionar las correlaciones.
8. Comparar SARIMA, Regresión Lineal, Random Forest, XGBoost y LSTM.
9. Relacionar los resultados publicados con la propuesta del Índice de Presión
   Turística.

## Orden recomendado de lectura

1. `data_etl.ipynb`: documenta el proceso de extracción y preprocesamiento de los datos empleados posteriormente en cada uno de los algortimos.
1. `sarima_code.ipynb`: baseline univariante, estacionalidad mensual y validación walk-forward one-step-ahead.
2. `lr_code.ipynb`: regresión lineal/Ridge sobre variables seguras y objetivo diferenciado.
3. `rf_code.ipynb`: ensamble no lineal optimizado con RandomizedSearchCV y GridSearchCV.
4. `xgboost_code.ipynb`: boosting multivariante y modelo de referencia para el IPT.
5. `lstm_code.ipynb`: secuencias diferenciadas, normalización y búsqueda de arquitectura recurrente.
