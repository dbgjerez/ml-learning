# Modelización predictiva

## Constraste de hipótesis
Vamos a defender siempre la hipótesis nula, en caso de que no se cumpla tendremos la hipótesis alternativa.

Un ejemplo de hipótesis nula serian los datos históricos, por ejemplo que en la ciudad de Sevilla la edad media sea menor de 40. En caso de que aumente, la hipótesis nula no será válida.

Según conozcamos la desviación típica o no tendríamos un ```z-test``` o un ```t-test```.

Para realizar un contraste de hipótesis:
* Definir la hipótesis nula en base al histórico o un valor conocido.
* Tomar una pequeña parte y calcular promedio, etc.
* Calcular el z-valor o el t-valor.
* Calcular el p-valor asociado
* Comprar p-valor y nivel de significación

## Correlación
La correlación lo que hace es buscar una relación o dependencia entre variables.

La correlación nos da una relación entre varias variables y nos facilita la vida al predecir una salida. 

### Coeficiente de correlación
El coeficiente de Colleración de Pearson nos indica cómo de fuerte es esa correlación. 

El coeficiente tiene sentido entre [-1, -0.6] y [0.6, 1] según sea negativa o positiva. 

Pandas nos da automáticamente los coeficientes de correlación: 

```python
data.corr()
```

### Nube de puntos
La nube de puntos es una buena representación gráfica de la correlación entre variables.

### Matshow
Nos pintaría directamente una matriz de correlación y lo vemos rápidamente de un vistazo.

```python
plt.matshow(data.corr())
```

## Regresión lineal