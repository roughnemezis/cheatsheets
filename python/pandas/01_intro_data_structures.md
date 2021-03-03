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


# Dataframes

## Méthodes de création

### A partir d'un dict de Series ou de dicts
```python
d = {
     "one": pd.Series([1.0, 2.0, 3.0], index=["a", "b", "c"]),
     "two": pd.Series([1.0, 2.0, 3.0, 4.0], index=["a", "b", "c", "d"]),
 }
pd.DataFrame(d)
# retourne: 
#    one  two
# a  1.0  1.0
# b  2.0  2.0
# c  3.0  3.0
# d  NaN  4.0
pd.DataFrame(d, index=["d", "b", "a"], columns=["two", "three"])
#    two three
# d  4.0   NaN
# b  2.0   NaN
# a  1.0   NaN
```
* l'index est la réunion des index des séries
* si nested dict, ils sont transformés en Series préalablement
* si argument index est passé, seuls les index y figurant sont dans le df, et une ligne vide (NaN) est créée pour tout index ne figurant pas dans le dict
* (même comportement pour l'argument columns)

### A partir d'un dict de ndarrays ou de listes
Contraintes (cohérentes):
* arrays ou listes tous de la même longueur
* si un index est passé il doit être de la même longueur
```python
pd.DataFrame({"one": [1.0, 2.0, 3.0, 4.0], "two": [4.0, 3.0, 2.0, 1.0]}, index=["a","b","c","d"])
#    one  two
# a  1.0  4.0
# b  2.0  3.0
# c  3.0  2.0
# d  4.0  1.0
```

### Autres méthodes

```python
pd.DataFrame([{"a": 1, "b": 2}, {"a": 5, "b": 10, "c": 20}]) # from list of dicts
#    a   b     c
# 0  1   2   NaN
# 1  5  10  20.0
pd.DataFrame(
     {
         ("a", "b"): {("A", "B"): 1, ("A", "C"): 2},
         ("a", "a"): {("A", "C"): 3, ("A", "B"): 4},
         ("a", "c"): {("A", "B"): 5, ("A", "C"): 6},
         ("b", "a"): {("A", "C"): 7, ("A", "B"): 8},
         ("b", "b"): {("A", "D"): 9, ("A", "B"): 10},
     }
 ) # depuis un dict de tuples (crée un multiindex)
#  a              b      
#       b    a    c    a     b
# A B  1.0  4.0  5.0  8.0  10.0
#  C  2.0  3.0  6.0  7.0   NaN
#  D  NaN  NaN  NaN  NaN   9.0
```
et il en reste (voir la doc pour plus d'informations):
* à partir d'une Series 
* à partir d'une liste de namedtuples
* à partir d'une listes de dataclasses
* avec méthodes pd.DataFrame.from_dict et pd.DataFrame.from_records

## Manipulation de colonnes

Similaire à la manipulation de dictionnaires, à noter:
* méthode pop pour récupérer et supprimer la colonne du df
* une valeur scalaire est propagée pour remplir la colonne ```df["foo"] ='bar'```
* insertion d'une série avec index ne matchant pas le df: elle est mise en conformité avec l'index du df(avec des Nan)
* * méthode d'insertion à un emplacement donné d'une colonne: ```df.insert(1,"bar", df["one"])```


## Assignement de colonnes pour chaînage
```python
import plotly.express as px
iris = px.data.iris()
```
Assignement d'une nouvelle colonne (retourne un nouveau df l'ancient est toujours inchangé)
```python
iris.assign(sepal_ratio=iris["sepal_width"] / iris["sepal_length"])
```
En utilisant une fonction (utile pour chaîner les opérations quand on ne connaît pas le nom du dataframe)
```python
iris.assign(sepal_ratio=lambda x: (x["sepal_width"] / x["sepal_length"]))
```
Exemple d'utilisation
```python
 (
     iris.query("sepal_length > 5") # équivaut à: iris[iris.sepal_length > 5]
     .assign(
         sepal_ratio=lambda x: x.sepal_width / x.sepal_length,
         petal_ratio=lambda x: x.petal_width / x.petal_length,
     )
     .plot(kind="scatter", x="sepal_ratio", y="petal_ratio")
 )
 ```
 
 
 ## Indexing / selection


Operation | Syntax | Result
--------- | ------ | ------
Select column | df[col] | Series
Select row by label | df.loc[label] | Series
Select row by integer location | df.iloc[loc] | Series
Slice rows | df[5:10] | DataFrame
Select rows by boolean vector | df[bool_vec] | DataFrame

## Alignement de données et arithmétique

* dans le résultat d'une opération arithmétique entre 2 DataFrames, l'alignement se fait automatiquement sur les lignes et les colonnes (le résultat aura l'union des index et columns comme index et columns respectivement)
* c'est vrai pour les opérations arithmétiques ```+,-,*,**,/```
* on peut appliquer les opérateurs booleens bitwise de python ```~,&,|,^``` pour appliquer des opérateurs booléens à un ou deux tableaux **de booléens** (```not, and, or, xor```). Voir à ce sujet [ce lien](https://stackoverflow.com/questions/21415661/logical-operators-for-boolean-indexing-in-pandas)
* des opérations avec des scalaires s'appliquent à tout le tableau
* lors d'une opération entre un DataFrame et une Series, par défaut l'index de la série est aligné sur les colonnes du DataFrame


## Interopérabilité avec numpy

Toutes les ["ufunc Numpy"](https://numpy.org/doc/stable/reference/ufuncs.html#available-ufuncs) peuvent être appliquées aux DtaFrames et aux Series, avec conservation de l'alignement de l'index
```python
np.exp(df) # par exemple
```

