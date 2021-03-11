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
