# Read the data

A class example to read the data from a directory.

```python
import os
import sys
import pandas as pd
from enum import Enum

class DataType(Enum):
    TRAIN = "train.csv"
    TEST = "test.csv"

class LoaderData():
    def __init__(self, path="."):
        self.path = path
        
    def load_data(self, data_type: DataType, index_col="Id"):
        return pd.read_csv(self.path + "/" + data_type.value, index_col=index_col)
```

Use example: 

```python
df_train = LoaderData(path="/kaggle/input/titanic/").load_data(DataType.TRAIN, index_col="PassengerId")
```

