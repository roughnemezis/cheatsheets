# Références

Documentation: https://plotly.com/python/

Référence de l'API: https://plotly.com/python-api-reference/



# Plotly Express

## Types de graphiques possibles: (TBC...)

* px.scatter
```python
px.scatter(x=[0, 1, 2, 3, 4], y=[0, 1, 4, 9, 16])
px.scatter(df, x="sepal_width", y="sepal_length")
px.scatter(df, x="sepal_width", y="sepal_length", color="species",size='petal_length', hover_data=['petal_width'])
```
* px.line

## Paramètres utiles des fonctions de tracé plotly express
(à mettre sous forme de tableau?)

* labels (intitulés axes x et y)
```python
fig = px.line(x=t, y=np.cos(t), labels={'x':'t', 'y':'cos(t)'})
```



# Besoins courants

## Tracer sur un même graphique des données avec des axes y différents
[Documentation](https://plotly.com/python/multiple-axes/)

```python
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# Create figure with secondary y-axis
fig = make_subplots(specs=[[{"secondary_y": True}]])

# Add traces
fig.add_trace(
    go.Scatter(x=[1, 2, 3], y=[40, 50, 60], name="yaxis data"),
    secondary_y=False,
)

fig.add_trace(
    go.Scatter(x=[2, 3, 4], y=[4, 5, 6], name="yaxis2 data"),
    secondary_y=True,
)
# etc...
```


# go.Scatter

## Exemple d'utilisation:
```python
import plotly.graph_objects as go
import numpy as np
N = 1000
t = np.linspace(0, 10, 100)
y = np.sin(t)
fig = go.Figure(data=go.Scatter(x=t, y=y, mode='markers'))
fig.show()
```

go.Scattergl version s'appuyant sur webgl, plus rapide pour les plots contenant beaucoup de points (mêmes arguments).

## Arguments

Attribut |Valeurs|Commentaire
--- |---|---
**mode** | `lines` `markers` `lines+markers` | Tracé sous forme de points / lignes / les deux
**marker** | `go.scatter.Marker` | Style du marqueur (attributs utiles: color, size)
**text** | `'Mon texte'` `df.somecolumn` | Texte à afficher lors hoover


# go.scatter.Marker

Attribut |Valeurs|Commentaire
--- |---|---
**color** | `df.somecolumn`  `'rgba(152, 0, 0, .8)'`|
**colorscale** | `'Viridis'` | [infos sur les colorscales](https://plotly.com/python/colorscales/)
**size** | `df.somecolumn` |
**symbol** | `circle, circle-open, 202` | [Liste complète des symboles](https://plotly.com/python/marker-style/#custom-marker-symbols)

Pour les colorscales voir: https://plotly.com/python/colorscales/
