# Entorno
* Anaconda
* Python
* Utilizar Google colab
* Uso de environment.yaml en caso de trabajar en local con entornos virtuales
* Usar en local jupyter

## Librerias
* **pandas:** librería para leer datasets y trabajar con ellos.

## GitHub de referencia
* https://github.com/joanby/python-ml-course/tree/master

# Data Science
## Disciplinas
* **Estadística**
* **Algormitmos**
* **Herramientas, técnicas y lenguajes de progración**
* **Contexto empresarial**

## Etapas
* **Etapa 1:** toma de requisitos de negocio
* **Etapa 2:** preparar los datos
* **Etapa 3:** explorar los datos
* **Etapa 4:** modelar y evaluar
* **Etapa 5:** comunicación y puesta en producción

## Tipos
* **Análisis retrospectivo:** en base al histórico
* **Análisis predictivo:** predicción en base al pasado

### Modelos predictivos
Conjunto de algoritmos estadísticos que dado un conjunto de **datos históricos**, devuelven una **función matemática** útil en un determinado contexto de tiempo y ámbito. 

Tipos de algoritmos estadísticos
* **Algoritmos supervisados:** datos históricos tienen una variable de salida. 
* **Algoritmos no supervisados:** no necesitan histórico.

Herramientas de estadísticas a usar Open Source: 
* **R**
* **Pythib**

La construcción del modelo:
* Sobre datos pasados
* Devisión en dos conjuntos:
    * Entrenamiento
    * Validación
* Buscamos crear una función matemática o ecuación que represente la predicción

La limpieza del dato será el 80% del tiempo. Es la tarea más complicada. 

# Preparación de los datos
Una vez preparados los datos es típico sacar estadística y diferentes métricas de lo smismos para asegurarnos de que está todo correcto y podemos empezar a realizar el modelo.

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

## Datos inválidos o datos que faltan
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

### ¿Qué hacer con datos que faltan?
#### Borrado de datos que faltan
Podemos borrar toda la fila o toda la columna haciendo uso de la función ```dropna```. Normalmente borraremos filas cuando falten datos y borraremos columnas cuando no haya suficientes datos, por ejemplo que el x% de los datos de la columna sean ```NaN```

```python
data.dropna(axis=0, how=all)
```

* **how=all:** solo borra si todo es ```NaN```, es decir, todas las columnas de la fila. 
* **how=any:** borra la fila si cualquier dato de la columna es ```NaN```.
* **axis=0:** borra la fila
* **axis=1:** borra la columna

#### Cómputo de los datos faltantes
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

## Variables categóricas
Un claro ejemplo es el sexo, podemos tener una columna que sea ```sex``` pero nos interese, tener dos columnas, una llamada ```male``` y otra ```female```. De este modo podemos rellenar con ceros y unos según pertenezca o no a la categoria. 

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

## Visualización del dataset
Para visualizar en línea los diagramas se utiliza:

```python 
% matplotlib inline
```

Para guardar una gráfico o imagen, utilizamos: 

```python
savefig("path.png/jpeg")
```

### Scatter plot
Es una nube de puntos. Se utiliza mucho cuando hay relación o dependencia entre dos columnas.

```python
data.plot(kind="scatter", x="column_a", y="column_b")
```

De esta manera se realizan representaciones básicas, si queremos comparar varias columnas, se utilizan paneles:

```python
import matplotlib.pyplot as plt
figure, axs = plt-subplots(2,2, sharey=True, sharex=True)
data.plot(king="scatter", x="column_a", y="column_b", ax=axs[0][0])
data.plot(king="scatter", x="column_b", y="column_c", ax=axs[0][1])
data.plot(king="scatter", x="column_a", y="column_c", ax=axs[1][0])
data.plot(king="scatter", x="column_a", y="column_d", ax=axs[1][1])
```