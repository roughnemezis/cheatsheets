Voir le tutoriel officiel de pandas très complet: https://pandas.pydata.org/pandas-docs/dev/user_guide/dsintro.html

On reprend ici en résumé quelques éléments

# Séries

```python
import numpy as np
import pandas as pd
```

## Méthodes de création:

```python
s = pd.Series(data, index=index)
```
data peut-être:
* un ndarray (tableau numpy)
```python
s = pd.Series(np.random.randn(5), index=["a", "b", "c", "d", "e"])
```
* un dictionnaire (les clés sont utilisées comme index
```python
s = pd.Series({"b": 1, "a": 0, "c": 2})
```
* un scalaire (dans ce cas l'index est obligatoire). La valeur est répétée pour tous les index
```python
s = pd.Series(5.0, index=["a", "b", "c", "d", "e"])
```

## Les Series comme ndarray

La plupart des fonctions numpy peuvent prendre une Series comme argument. La  "Series" conserve son index (sauf sur des opérations réalisant un slicing).
```python
s[0]       # 0 est une position et pas un index (suaf s'il est entier...)
s[:3]      # même remarque
s[s > s.median()]
s[[4, 3, 1]]
np.exp(s)
```

L'accès est également possible par valeur de l'index ("dict-like")

## Opérations vectorisées

```python
s+s
s*2
np.exp(s)
s[1:]+s[:-1]
```
Les opérations entre series **alignent automatiquement les index**, on n'a pas à s'en soucier.
L'index de serie1 + serie2 sera la réunion des deux index, et lorsque l'index n'existe que dans un tableau la
valeur résultante est "NaN".



