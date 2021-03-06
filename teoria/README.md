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

### Concatenación de datos
Normalmente la fuente de datos procede de varios datasets, por tanto es importante poder generar un conjunto de datos único. 

Para la combinación de datasets tenemos que tener en cuenta el tipo de eje:

* axis = 0 denota el eje horizontal
* axis = 1 denota el eje vertical

La librería pandas nos facilita la concatenación:

```python
res_data = pd.concat([data_a, data_b], axis = 0)
```

En ocasiones tenemos cientos de csv, por ejemplo uno por día, uno por sensor, etc.

En este caso será necesario unificar o seguir un determinado nombre dentro de los ficheros ya que serán importados en un bucle y se realizará un concatenado sobre el primero. 

### Join de datasets
El join de dos datasets se realiza de forma similar a SQL, es decir, a través de una clave única se cruzan los datos.

#### Merge
La librería pandas nos facilita una función de merge, la cuál nos une dos dataset a través de una clave principal, esto sería similar a un ```inner join```, por tanto se puede producir un duplicado de filas en caso de existir claves repetidas en algunos de los datasets.

Para este tipo de operación, es importante tener ficheros de unión de 1:1, en vez de 1:n, por tanto se puede realizar una limpieza en el fichero de unión, haciendo uso de la función ```drop_duplicates```

```python
data = data.drop_duplicates(subset="User")
```

Para realizar los diferentes tipos de merge se indicara con el comando ```how``` de la función ```merge```
```python
merged_data = pd.merge(left = "data_a", right = "data_b", how = "inner")
```

#### Left join

![Left join](images/left-join.png)

#### Inner join
![Inner join](images/inner-join.png)

#### Right join
![Right join](images/right-join.png)

#### Outer join
Nos devuelve lo que tenga A y B.