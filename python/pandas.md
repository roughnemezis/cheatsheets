## Calculer le minimum de deux séries

Il y a des solutions en passant par numpy mais il est plus intéressant de rester dans le monde pandas pour bénéficier de l'index.

```python
# S1 et S2 sont deux séries partageant un index commun
pd.DataFrame([S1, S2]).min()
```
