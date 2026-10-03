# Informes y resultados

La memoria académica que motiva este repositorio (`TFM_AdrianSuarezSixto.pdf`) no se incluye aquí. El
repositorio está diseñado para explicar y reproducir el flujo de análisis a
través del cuaderno anotado y de las referencias a fuentes públicas de
`data/README.md`.

El informe compara cinco modelos mediante MAPE, RMSE, MAE y R2, y añade la precisión de clasificación del Índice de Presión Turística. El resultado operativo destacado es XGBoost como modelo multivariante, con 17 de 18 niveles IPT correctamente clasificados.

La trazabilidad del análisis se puede seguir desde `/notebooks`: allí se
documentan la extracción y transformación de los datos, la limpieza, preparación
de las fuentes externas e ingeniería de características así como la ejecución independiente de cada algoritmo.

Las principales conclusiones de la memoria se resumen en el `README.md` raíz.
