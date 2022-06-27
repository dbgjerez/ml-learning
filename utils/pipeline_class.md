# Pipeline

## Fill nulls or invalid values

Class to fill the null values with a specific strategic.

```python
from sklearn.base import BaseEstimator, TransformerMixin

class CustomImputer(BaseEstimator, TransformerMixin):
    def __init__(self, imputer, strategy, fill_value=0):
        self.imputer = imputer
        self.strategy = strategy
        self.fill_value = fill_value
    def fit(self, X, y=None):
        self.imputer = self.imputer(strategy=self.strategy, fill_value=self.fill_value)
        self.imputer.fit(X, y)
        return self
    def transform(self, X):
        X_imp_tmp = self.imputer.transform(X)
        X_imputer = pd.DataFrame(X_imp_tmp, index=X.index, columns=X.columns)
        return X_imputer
```

We have many options to use this function

### Categorical data
To fill, for example with 'NaN' category: 

```python
cat_transformer = Pipeline(steps=[
    ('cat_imputer', CustomImputer(SimpleImputer, strategy='constant', fill_value='NA')),
])
```

### Numeric data

When we use a numeric data we can fill the null values with some different strategies: ```median, most_frequent, constant or mean```

```python
num_transformer = Pipeline(steps=[
    ('num_imputer', CustomImputer(SimpleImputer, strategy='median')),
])
```