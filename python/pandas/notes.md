# Notes à compléter

## Booleens et shift


"shift" appliqué à une série de booléens renvoie une série de dtype 'object'
en effet, Nan est utilisé pour compléter les valeurs manquantes, et Nan est compatible avec les types numériques (à creuser)

solutions possibles:
- utiliser astype('bool') après conversion
- utiliser l'argument fill_value de shift (fill_value=False par exemple)

TODO : creuser la question des dtypes en pandas
[lien vers le tuto](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#dtypes)


## Exprimer des intervalles de temps

(par exemple pour comparaison avec une durée)
```python
pd.Timedelta("3 hours 20 minutes 10 seconds")
```

## Générer un index de dates

```python
pd.date_range(start="2018-08-01", end ="2019-08-01", freq='1D')
```
pour la liste des alias pour définir les fréquences voir [ce tableau](https://pandas.pydata.org/docs/user_guide/timeseries.html?highlight=offset%20aliases#offset-aliases) dans la doc (par exemple '10min' pour 10 minutes).

Autre arguments utiles : periods, name (voir la doc)
