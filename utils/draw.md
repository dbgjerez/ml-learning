# Draw and visualize the data

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
import seaborn as sns

# Set Matplotlib defaults
plt.style.use("seaborn-whitegrid")
plt.rc("figure", autolayout=True)
plt.rc(
    "axes",
    labelweight="bold",
    labelsize="large",
    titleweight="bold",
    titlesize=14,
    titlepad=10,
)

class Draw():
```

## Correlation

The following function define the way of draw the correlation between y axis and differents variable.

```python
def correlation(self, df, y):
    num_cols = df.select_dtypes(exclude=['object'])
    correlation = num_cols.corr()
    corr_price = correlation[[y]]
    
    plt.figure(figsize=(17,20))
    for i in range(len(num_cols.columns)):
        plt.subplot(10,4,i+1)
        sns.scatterplot(x=num_cols.iloc[:, i], y=y, data=num_cols.dropna())
        plt.title('Corr to ' + y + ' = {}'.format(np.around(corr_price.iloc[i, 0], decimals=2)))
    plt.tight_layout()
    plt.show()
```

## Missing data

Function that return a table with a total of missing values and the percent of them respect to the total. 

```python
 def missing_data(self, df):
        missing = df.isnull().sum().sort_values(ascending=False)
        total_missing = missing[missing != 0]
        percent_missing = np.around(((missing / len(df) * 100)[(missing / len(df) * 100) != 0]), decimals=2)
        return pd.concat([total_missing, percent_missing], axis=1, keys = ['Total', 'Percent'])
```