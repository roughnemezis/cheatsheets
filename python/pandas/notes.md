# Notes à compléter

"shift" appliqué à une série de booléens renvoie une série de dtype 'object'
en effet, Nan est utilisé pour compléter les valeurs manquantes, et Nan est compatible avec les types numériques (à creuser)

solutions possibles:
- utiliser astype('bool') après conversion
- utiliser l'argument fill_value de shift (fill_value=False par exemple)

TODO : creuser la question des dtypes en pandas
[lien vers le tuto](https://pandas.pydata.org/pandas-docs/stable/user_guide/basics.html#dtypes)

