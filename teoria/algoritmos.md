# Algoritmos ML

## Supervisados

### Clasificación

#### XGBClassifier
#### DecisionTreeClassifier
```python
    import optuna
    optuna.logging.set_verbosity(optuna.logging.WARNING)

    def objective(trial):
        parameters = dict(
            learning_rate=trial.suggest_float("learning_rate", 0.01, 0.3),
            min_samples_split=trial.suggest_int("min_samples_split", 2, 4),
            min_samples_leaf=trial.suggest_int("min_samples_leaf", 1, 3),
            max_depth=trial.suggest_int("max_depth", 2, 4),
            max_features=trial.suggest_categorical("max_features",['auto','sqrt','log2']),
            subsample=trial.suggest_float("subsample", 0.6, 1.0),
            n_estimators=trial.suggest_int("n_estimators", 1, 250),
        )
        gbc = GradientBoostingClassifier(**parameters)
        return score_dataset(df_train_prepared, y, gbc)

    study = optuna.create_study(direction="maximize")
    study.optimize(objective, n_trials=250)
    gbc_best_params = study.best_params
    study.best_trial
```
#### RandomForestClassifier
#### ExtraTreesClassifier
#### GradientBoostingClassifier
#### KNeighborsClassifier
#### LogisticRegression
#### LinearDiscriminantAnalysis
#### SVC

## No supervisados