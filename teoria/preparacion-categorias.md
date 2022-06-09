# Variables categóricas
Las variables categóricas son un paso más en nuestro paso de preparación de los datos. 

Un claro ejemplo es el sexo, podemos tener una columna que sea ```sex``` pero nos interese, tener dos columnas, una llamada ```male``` y otra ```female```. De este modo podemos rellenar con ceros y unos según pertenezca o no a la categoria. 

## Pandas get_dummies
En python la libreria pandas tenemos la función ```get_dummies``` que se utiliza para esto:

```python
dummy_sex = pd.get_dummies(data["sex"], prefix="sex")
```

```dummy_sex``` contendrá un dataframe con un subconjunto de las variables dummy, ahora la idea sería concatenar al dataframe que tenemos: 

```python
data = pd.concat([data, dummy_sex], axis = 1)
```

Todo en una función definitiva sería de la siguiente forma: 

```python
def createDummies(df, var_name):
    dummy = pd.get_dummies(df[var_name], prefix="var_name")
    df = df.drop(var_name, axis = 1)
    df = pd.concat([df, dummy], axis = 1)
    return df
```

## OrdinalEncoder
En este caso a cada tipo o variable de la categoria le damos un valor, un número entero. 

Es posible que nuestro conjunto de entrenamiento sea diferente a nuestro conjunto de validación, es decir, si lo hacemos por separado, no tiene porque coincidir los números de orden. Para evitar esto se utiliza la libreria ```OrdinalEncoder``` del paquete sklearn.

```python
ordinal_encoder = OrdinalEncoder()
label_X_train[good_label_cols] = ordinal_encoder.fit_transform(X_train[good_label_cols])
label_X_valid[good_label_cols] = ordinal_encoder.transform(X_valid[good_label_cols])
```