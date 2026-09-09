# Figuras

Este directorio está reservado para las figuras generadas por el cuaderno. Las
imágenes generadas se ignoran mediante Git para que el repositorio no contenga
por defecto archivos derivados.

El cuaderno produce visualizaciones para:

- La serie histórica de pernoctaciones.
- La descomposición estacional y los diagnósticos ACF/PACF.
- La matriz de correlación de las variables.
- La comparación entre predicciones y observaciones del conjunto de prueba.

`notebooks/data_etl.ipynb` genera las figuras exploratorias y de diagnóstico del
preprocesamiento, incluida la evolución de la serie, la descomposición
estacional, ACF/PACF y la matriz de correlación. Sus bloques de algoritmo usan
el estilo visual de los gráficos adjuntos al informe. Los notebooks individuales
mantienen su propia salida PNG para permitir reproducir cada modelo sin ejecutar
todo el cuaderno de preparación.
