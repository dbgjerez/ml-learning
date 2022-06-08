# Preparación de los datos
Uno de los puntos más importantes es la preparación de los datos, para tal fin se suele realizar en dos pasos, en uno primero detectamos posibles problemas, en el siguiente paso los solventamos con diferentes estrategias. 

## Resumen de los datos

### Visualización previa:
Ver n datos primeros datos
```python
data.head(10)
```

Ver los n últimos datos
```python
data.tail(8)
```

### Dimensión
Es el tamaño, filas, columnas, etc.
```python
data.shape
```

Podemos ver también las columnas que tiene:
```python
data.columns.values
```

El tipo de las columnas:
```python
data.dtypes
```

### Resumen estadístico básico

```python
data.describe()
```

## Datos inválidos o nulos
Al hacer el describe por ejemplo, podemos ver que faltan datos. Porque el count, número de filas, no coincide con el ```count``` de las columnas.

Existe una función que nos dice los valores que son nulos.

```python
pd.isnull(data["body"])
```

También tenemos la función ```notnull``` que hace lo contrario, esd ecir, decirnos cuales no son nulos.

Para contar los datos que son nulos:
```python
data.isnull(data["body"]).values.ravel().sum()
```

> NOTE: ravel() normaliza el array en una sola dimensión

Pueden venir valores nulos por dos motivos:
* Falta a la hora de la extración de los datos: al exportar
* Recolección de los datos: ni siquiera los tenemos en la base de datos, no hemos podido conseguirlos

### Borrado de datos nulos o inválidos
Podemos borrar toda la fila o toda la columna haciendo uso de la función ```dropna```. Normalmente borraremos filas cuando falten datos y borraremos columnas cuando no haya suficientes datos, por ejemplo que el x% de los datos de la columna sean ```NaN```

```python
data.dropna(axis=0, how=all)
```

* **how=all:** solo borra si todo es ```NaN```, es decir, todas las columnas de la fila. 
* **how=any:** borra la fila si cualquier dato de la columna es ```NaN```.
* **axis=0:** borra la fila
* **axis=1:** borra la columna

### Cómputo de los datos faltantes
Se sustituyen los ```NaN``` por un valor concreto. Existen muchos métodos.

Utilizaremos la función ```fillna```, esta función rellena los campos ```NaN``` con los valores que indiquemos. Podemos aplicar a todo el dataset o a una sola columna. 

```python
data = data["city"].fillna("Desconocido")
data = data.fillna("Desconocido")
```

Existen diversas estrategias: 

* **Rellenar con 0/texto:** no es muy buena, puesto que no tiene sentido en todos los casos.
* **Promedio/meadiana,etc:** se rellena las columnas con el valor del promedio. por ejemplo: ```data["age].fillna(data["age"].mean())```

Parámetros de fillna:

* **ffill:** es forward fill, rellena con los valores más próximos hacia delante: ```data["age"].fillna(method="ffill")```
* **backfill:** es back fill, rellena con los valores más próximos hacia detrás: ```data["age"].fillna(method="backfill")```

### SimpleImputer
La función SimpleImputer de la libreria sklearn nos facilita el rellenado de datos con valores específicos. Por defecto se utilizará la media:

Es muy importante utilizar la función ```fit_transform``` para el dataset de entrenamiento y ```transform```para el dataset de validación.

```python
from sklearn.impute import SimpleImputer

imputer=SimpleImputer() 
imputed_X_train = pd.DataFrame(imputer.fit_transform(X_train))
imputed_X_valid = pd.DataFrame(imputer.transform(X_valid))
```