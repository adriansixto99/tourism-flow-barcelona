# Predicción del flujo turístico en Barcelona

Repositorio de análisis de datos para el proyecto **Modelos de Inteligencia
Artificial para la predicción del flujo turístico en destinos urbanos**.

El proyecto estudia si es posible predecir con suficiente precisión las
pernoctaciones turísticas mensuales en Barcelona para apoyar una planificación
más proactiva del turismo y de los servicios urbanos. Para ello se comparan un
modelo estadístico estacional, varios modelos clásicos de aprendizaje automático
y una red neuronal recurrente.

## Pregunta de negocio

¿Se pueden utilizar la demanda turística histórica así como otras variables externas (actividad aeroportuaria,
el clima, el calendario y el interés de búsqueda en Internet) para anticipar la
presión turística mensual en Barcelona?

La variable objetivo es el número mensual de pernoctaciones turísticas. Las
pernoctaciones se utilizan porque representan una presión sostenida sobre el
alojamiento, las infraestructuras y los servicios públicos más directamente que
el simple número de visitantes.

## Objetivos

- Construir una serie mensual limpia a partir de estadísticas oficiales de
  alojamiento.
- Integrar fuentes heterogéneas relacionadas con el turismo.
- Crear variables que representen la estacionalidad, la historia reciente y
  acontecimientos excepcionales.
- Comparar los modelos SARIMA, Regresión Lineal, Random Forest, XGBoost y LSTM.
- Evaluar tanto el error numérico de las predicciones como el rendimiento de las
  alertas operativas.
- Transformar las predicciones en una propuesta de Índice de Presión Turística
  (IPT).

## Estructura del repositorio

```text
tourism-flow-barcelona/
├── README.md
├── notebooks/
│   ├── lr_code.ipynb
│   ├── lstm_code.ipynb
│   ├── rf_code.ipynb
│   ├── sarima_code.ipynb
│   ├── xgboost_code.ipynb
│   ├── data_etl.ipynb
│   └── ANNOTATION_PLAN.md
│   └── README.md
├── data/
│   └── README.md
├── reports/
│   └── README.md
├── figures/
│   └── README.md
├── requirements.txt
└── .gitignore
```

## Flujo de trabajo analítico

1. **Definir la variable objetivo**: obtener las pernoctaciones mensuales de
   Barcelona a partir de la Encuesta de Ocupación Hotelera del INE.
2. **Limpieza de la variable objetivo**: filtrar las pernoctaciones, convertir los
   valores numéricos formateados, interpretar las fechas mensuales, ordenar
   cronológicamente y conservar los datos desde 2015.
3. **Recopilar señales externas**: integrar datos meteorológicos de AEMET,
   calendario de Cataluña, llegadas aeroportuarias de AENA e indicadores de Google
   Trends.
4. **Crear el dataset maestro**: alinear todas las fuentes mediante un índice
   mensual y conservar los meses presentes en todas ellas.
5. **Crear nuevas variables**: añadir retardos, medias móviles, codificación
   cíclica del mes y un indicador del shock de COVID-19.
6. **Dividir cronológicamente**: reservar los últimos 18 meses como conjunto de
   prueba para evitar aprender información del futuro.
7. **Entrenar y comparar modelos**: evaluar un modelo estacional univariante
   frente a métodos multivariantes de aprendizaje automático y Deep Learning.
8. **Interpretar operativamente**: clasificar la presión turística prevista en
   niveles verde, naranja o rojo mediante umbrales percentiles.

## Fuentes de datos

| Fuente | Uso en el análisis |
|---|---|
| [INE](https://www.ine.es/) | Variable objetivo: pernoctaciones mensuales y viajeros |
| [AEMET OpenData](https://opendata.aemet.es/) | Temperatura media y precipitación acumulada mensual |
| [AENA](https://www.aena.es/) | Llegadas mensuales al aeropuerto de Barcelona-El Prat y crecimiento |
| [Google Trends](https://trends.google.es/) | Interés de búsqueda sobre vuelos, hoteles y Airbnb en Barcelona |
| [datos.gob.es](https://datos.gob.es/) | Información de calendario, festivos y días no laborables |

Los archivos de datos no se incluyen. Consulta `data/README.md`.

## Ingeniería de características

El dataset de modelado descrito en el proyecto contiene aproximadamente 130
observaciones mensuales y 25 variables después de las transformaciones.

- **Retardos**: representan efectos diferidos de las llegadas aeroportuarias,
  el interés de búsqueda y las pernoctaciones recientes.
- **Variables móviles**: resumen el contexto reciente del clima y del interés de
  búsqueda.
- **Codificación cíclica del mes**: representa la continuidad entre diciembre y
  enero mediante seno y coseno.
- **Variables de calendario**: cuentan festivos, fines de semana y días libres
  reales.
- **Indicador de COVID-19**: aísla aproximadamente el periodo de restricciones
  comprendido entre marzo de 2020 y junio de 2021.

## Diseño de evaluación

La división es cronológica, no aleatoria. Los últimos 18 meses se mantienen como
conjunto de prueba, lo que proporciona al menos un ciclo estacional anual
completo y seis meses adicionales. Este diseño es apropiado para predicción,
porque una división aleatoria podría permitir que observaciones futuras
influyeran en el entrenamiento.

Las métricas principales son:

- **MAPE**: error relativo medio, útil para comparar modelos en porcentaje.
- **MAE**: error absoluto medio expresado en pernoctaciones.
- **RMSE**: métrica que penaliza con mayor intensidad los errores grandes.
- **R²**: proporción de la variabilidad de la variable objetivo explicada por el
  modelo.

En MAPE, MAE y RMSE, los valores más bajos son mejores. En R², los valores más
altos son mejores.

## Resultados publicados

| Modelo | MAPE | RMSE | MAE | R2 | Aciertos IPT |
| --- | ---: | ---: | ---: | ---: | ---: |
| SARIMA | 5.13% | 163,023 | 138,659 | 0.9558 | 83.3% (15/18) |
| XGBoost | 6.05% | 213,365 | 173,528 | 0.9243 | 94.4% (17/18) |
| Regresión Lineal | 7.96% | 278,004 | 230,698 | 0.8715 | 83.3% (15/18) |
| Random Forest | 8.21% | 282,402 | 246,901 | 0.8674 | 83.3% (15/18) |
| LSTM | 8.91% | 292,853 | 253,362 | 0.8574 | 77.8% (14/18) |


## Notas de reproducibilidad

El cuaderno fue preparado originalmente para Google Colab y utiliza rutas de
Google Drive como `/content/drive/MyDrive/VIU/TFM/Data/`. Para ejecutarlo
localmente es necesario obtener los archivos de entrada documentados y adaptar
la ruta de datos al entorno local.

## Requisitos

Las dependencias principales aparecen en `requirements.txt`. El flujo utiliza
pandas, NumPy, Matplotlib, Seaborn, Requests, Holidays, scikit-learn,
statsmodels, pmdarima, XGBoost, TensorFlow y Jupyter.

Algunos proveedores de datos o APIs pueden requerir credenciales independientes
o cambiar el formato de sus respuestas con el tiempo.

## Alcance y limitaciones

- El estudio se centra en Barcelona y no debe generalizarse a otros destinos sin
  una nueva validación.
- La frecuencia mensual limita el número de observaciones disponibles para
  entrenar los modelos.
- La correlación identifica asociación, no causalidad.
- La disponibilidad y el momento de actualización de los datos externos afectan
  al posible uso operativo.
- COVID-19 se trata como un periodo de shock definido; otras perturbaciones
  futuras podrían requerir un tratamiento nuevo.
- El IPT es un indicador de alerta temprana propuesto, no una medida completa de
  todas las dimensiones sociales, ambientales o infraestructurales de la presión
  turística.
