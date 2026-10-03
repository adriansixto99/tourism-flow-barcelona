# Directorio de datos

Los datasets brutos y procesados se excluyen intencionadamente del repositorio.
El proyecto público documenta el flujo de análisis sin redistribuir archivos de
terceros ni exportaciones locales o personales.

## Archivos de entrada esperados

El cuaderno espera encontrar los siguientes archivos en la carpeta de datos
configurada:

- `2074.csv`: extracto de la encuesta de alojamiento del INE usado para crear la
  serie objetivo.
- `clima_barcelona_historico_final.csv`: datos meteorológicos mensuales de
  AEMET.
- `aena_barcelona_historico_limpio.csv`: datos mensuales limpios del aeropuerto
  de Barcelona.
- `google_trends_barcelona.csv`: indicadores mensuales de Google Trends.
- `calendario_mensual.csv`: variables mensuales de festivos y fines de semana.

## Productos intermedios

El notebook `notebooks/data_etl.ipynb` genera o utiliza también estos productos del
flujo de preparación:

- `pernoctaciones_limpio.csv`: serie objetivo mensual limpia a partir de `2074.csv`.
- `aena_barcelona_historico_limpio.csv`: histórico mensual de llegadas aeroportuarias.
- `calendario_mensual.csv`: agregación mensual de festivos, fines de semana y días libres.
- `DATASET_TFM_FINAL.csv`: unión mensual de pernoctaciones y fuentes externas.
- `DATASET_TFM_AVANZADO.csv`: dataset con variables derivadas para modelado.
- `matriz_correlacion_features.png`: diagnóstico de correlaciones generado antes del modelado.


## Fuentes

- [INE](https://www.ine.es/)
- [AEMET OpenData](https://opendata.aemet.es/)
- [AENA](https://www.aena.es/)
- [Google Trends](https://trends.google.es/)
- [datos.gob.es](https://datos.gob.es/)


## Notas de preparación

Todas las fuentes deben convertirse a frecuencia mensual y alinearse mediante un
índice de fechas común. La variable objetivo es el número mensual de
pernoctaciones en Barcelona (`Pernoctaciones`). El dataset final de modelado
descrito en el TFM contiene aproximadamente 130 observaciones mensuales y 25
variables después de la ingeniería de características.
