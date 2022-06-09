# Cross-Validation
El proceso de cross-validation consiste en utilizar todo el dataset para entrenamiento y validación. Para ello se divide en n partes y se van analizando con la regla del 20-80 de forma que si se divide en 5, se realizan 5 experimentos.

Para medir la potencia se realizará la media de los valores de los resultados: 

```python
#cv = número de divisiones
scores = -1 * cross_val_score(my_pipeline, X, y,
                              cv=5,
                              scoring='neg_mean_absolute_error')

print("Average MAE score:", scores.mean())
```