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

Para dar un nombre al eje x se utiuliza ```plot.xlabel("txt")``` y para dar un nombre al eje y ```plot.ylabel("txt")```.

Para definir un título se usa la función ```plt.title("title")```

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

### Histogramas
Se visualizan los rangos de un solo vistazo. Se usa mucho para ver la distribución de los datos.

```python
plt.hist(data["column"], bins = 10)
```

```bins``` representa los rangos o "trocitos" en los que dividirlo. La ```Regla de Sturges``` nos indica el número de divisiones a realizar. Para aplicarla: ```int(np.ceil(1+np.log2(num_filas)))```

### Boxplot, diagrama de caja y bigotes
Afina el histograma. La caja central es donde se distribuyen los datos. En el cuartil del 25% empiezarían los datos y se terminaría en el cuartil de los 75%. Es decir, la caja representa el 50% de los datos. La línea central representa la mediana.

El bigote es el tamaño de la caja (cuartil 75 - cuartil 25) +- 1.5 veces el IQR (rango intercuartil). 

```python
plt.bxplot(data["column"])
```

## Data Weangling 
Es el proceso de transformación y manipulación de datos para hacerlos más "analizables". En ocasiones los datos se reciben en crudo, o se encuentran en n bases de datos, por tanto hay que realizar un proceso de preparar los mismos. 

### Crear un subconjunto de datos

#### Columnas
A veces existen demasiadas columnas y muchas no nos son necesarias, por tanto podemos limpiar dicho dataset. 

Para eliminar determinadas columnas:

```python
subset = data[["columna_a", "columna_b", "columna_c"]]
```

Otra forma es indicar cuales no quiero:

```python
all_columns = data.columns.values.tolist()
undesired_columns = ["a","b","c"]
sublist = [x for x in all_columns if x not in undesired_columns]
subset = data[sublist]
```

#### Filas

Filtrado de filas según índice: 

```python
subset = data[0:25]
```

Es mucho más útil hacer dicho filtrado con condicionales. Un ejemplo sería quedarnos con aquellos usuario con más de 18 años:

```python
subset = data[data["age"]>18]
```

#### iloc y loc
A veces nos interesa dar unas coordenadas de filas y columnas, para eso se utiliza dichas funciones.

Con ```iloc``` diremos qué filas y qué columnas queremos preservar.

```python
data.iloc[:10, 3:6] ## de la fila 0 a la 10
data.iloc[:10, :] ## 10 primeras filas y todas las columnas
```

Si queremos acceder por etiquetas o nombre de la columna, usaremos la función ```loc```.

```python
data.loc[:10, ["age"]] ## de la fila 0 a la 10 y columna age
```


### Generación de números aleatorios
La generación de números aleatorios generará un número diferente cada vez que se llaman.

La librería ```numpy``` es la librería que se suele utilizar para la generación de números aleatorios. 

```python
import numpy as np
np.random.randint(1,100) ## número aleatorio entre 1 y 100
np.random.random() ## número aleatorio con decimales entre 0 y 1
```

Una función muy útil para la generación de aleatorios, se encuentra en el paquete ```random```:

```python
import random
random.randrange(0,100)
```

Otro método muy importante es ```shuffle```, el cuál reordena de forma aleatoria, una especie de barajado de los elementos del array.

```python
np.random.shuffle(number_list)
```

Por último, la función ```choice``` nos selecciona un valor aleatorio de un lista

```python
np.random.choice(number_list)
```

La generación de números aleatorios, a veces nos interesa conservarla, por ejemplo para poder reproducir:

La función ```random``` tiene la opción de introducir la semilla:

```python
np.random.seed(2022)
np.random.random()
```

#### Distribución de probabilidades en generación de número aleatorios
Para la generación de datos se sigue dos tipos de distribuciones normalmente, la uniforme y la de Gauss.

###### Distribución uniforme
Este tipo de distribución hace que sea igual de probable cualquier numero dentro del rango.

```python
import numpy as np
a = 1
b = 100
n = 200
data = np.random.uniform(a, b, n)
```

Si queremos pintarlo para comprobar la distribución: 

```python
%matplotlib inline
plt.hist(data)
```

###### Distribución normal
La Distribución normal o la Campana de Gauss es la distribución que se suele dar en más del 98% de los casos de la vida. 

Conforme vamos añadiendo datos, nos vamos acercando a una distribución normal. 

La función ```randn``` nos genera números aleatorios siguiendo esta distribución. 

```python
data = np.random.randn(1000000)
```

Si los pintamos, podríamos ver la distribución de los valores:

```python
x = range(1,101)
plt.hist(x,data)
```

### Generación de Data Sets aleatorios

#### Datos numéricos

Para la generación de un dataframe aleatorio, usuaremos directamente el constructor de pandas ```pd.DataFrame()```:

```python
n = 1000
data = pd.DataFrame(
    {
        'A' : np.random.randn(n), ## distribución en una normal 0, 1
        'B' : 1.5 + 2.5 * np.random.randn(n), ## distribución según una normal en una media 1.5 con una desviación típica de 2.5
        'C' : np.random.uniform(5, 32, n) ## distribución uniforme entre 5 y 32
    }
)
```

#### Variables categóricas

Para la generación de datos según información categórica utilizaremos ```numpy``` y ```pandas```.

```python
n = 1000

gender = ["Male", "Female"]
gender_data = []

for i in range(0, n):
    gender_data.append(np.random.choice(gender))
age = 30 + 10 * np.random.rand(n)

data = pd.DataFrame(
    {
        "Gender": gender_data,
        "Age": age
    }
)
```

### Agrupación de datos por categorías 

Para la agrupación, utilizaremos la función ```groupby```, la cual funciona de forma similar al SQL.

Un ejemplo para agrupar por género en el punto anterior, sería: 

```python
grouped_gender = data.groupby("Gender")
grouped_gender.groups
grouped_gender.get_group("Female")
```

A los objetos agrupados se le pueden realizar las operaciones típicas de un DataFrame ya que son un subconjunto de datos, por ejemplo.

```python
grouped_gender.describe()
```

#### Aggregate

Si queremos realizar un agregado o una serie de operaciones sobre las agrupaciones, podemos utilizar la función ```aggregate```. Dicha función se indica los nombres de las columnas y la función que queramos aplicar. 

```python
data.aggregate(
    {
        "Age": np.mean,
        "Total": np.sum
    }
)
```

Otro ejemplo sería el uso de lambdas, el cual nos permite realizar una operación sobre una columna, por ejemplo, la fórmula de la velocidad es: ```v = e / t```

```python
data.aggregate(
    {
        "Speed": lambda v: e / t
    }
)
```

#### Filtrado
La función de filtrado va aplicando condiciones para ir eliminando datos de la salida:

```python
data["Age"].filter(lambda x: x > 18)
```

#### Transformación de variables
La transformación de variables se realiza definiendo una lambda y aplicando la misma al dataset o a una serie de columnas deseadas:

```python
zscore = lambda x : (x - x.mean()) / x.std()
z_group = group.transform(zscore)
```

La combinación de ```fillna``` junto a los ```transform``` nos ofrece una forma muy potente de rellenar datos que faltan.

```python
fill_na = lambda x: x.fillna(x.mean())
data.transform(fill_na)
```

#### Ordenación y consulta
Para consultar un elemento en una determinada fila, utilizaremos la función ```nth```, por ejemplo para el elemento en la posición 70: 

```python
data.nth(70)
```

Además, tenemos las funciones ```head(n)``` para visualiza los primero n elementos y la función ```tail(n)``` para visualizar los n últimos elementos. 

La ordenación de datos se realiza haciendo uso de la función ```sort``` por uno o varias columnas:

```python
data.sort_values(["Age"])
```

### División del conjunto de datos en entramiento y test

Normalmente se utiliza un porcentaje de 80%-20% de los datos para entrenar y para hacer el testing del modelo. 

#### División utilizando distribución normal
Quedarse con los primeros 80% de números generados de forma aleatoria:

```python
a = np.random.randn(len(data))
check = (a<0.8)
training = data[check]
testing = data[~check]
```

#### Haciendo uso de la librería sklearn
Librería estándar para la implementación de datos estadísticos. Con esta librería ya tenemos definidos los algoritmos necesarios:

```python
from sklearn.model_selection import train_test_split
train, test = train_test_split(data, test_size = 0.2)
```

#### Usando una función de shuffle
En este caso lo importante es que se "baraja" los datos y se puede reproducir modificando la semilla de random.

```python
import sklearn
data = sklearn.utils.shuffle(data)
cut_id = int(0.75 * len(data))
train_data = data[_cut_id]
test_data = data[cut_id+1:]
```
