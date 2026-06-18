## 1. Pourquoi la mise à l'échelle est-elle indispensable ?

Dans un jeu de données réel, les *features* ont souvent des ordres de grandeur très différents (ex: l'âge varie de 0 à 100, le salaire de 10 000 à 100 000). Si l'on fournit ces données brutes à un algorithme, deux problèmes majeurs surviennent :

1. **Biais des algorithmes basés sur la distance :** Les modèles utilisant des distances géométriques (KNN, K-Means, SVM, PCA) accorderont mathématiquement un poids disproportionné à la variable ayant la plus grande échelle (le salaire écrasera totalement l'âge dans le calcul de la distance euclidienne).

2. **Lenteur de convergence :** Pour les algorithmes utilisant la Descente de Gradient (Régression Linéaire/Logistique, Réseaux de Neurones), des échelles hétérogènes créent une fonction de coût très allongée (en forme de vallée étroite). Le gradient va osciller de manière inefficace, ralentissant drastiquement la convergence vers le minimum.

L'objectif de la mise à l'échelle est de ramener toutes les *features* dans des proportions comparables sans altérer les relations mathématiques entre les observations.

## 2. La Normalisation (Min-Max Scaling)

La normalisation ramène strictement toutes les valeurs d'une variable dans un intervalle défini, généralement $[0, 1]$.

**Formule mathématique :**
$$X_{norm} = \frac{X - X_{min}}{X_{max} - X_{min}}$$

* **Propriétés :** La forme exacte de la distribution d'origine est préservée.

* **Vulnérabilité :** Extrêmement sensible aux valeurs aberrantes (*outliers*). Si $X_{max}$ est une valeur aberrante gigantesque, toutes les autres valeurs normales seront écrasées dans un espace minuscule proche de $0$.

* **Cas d'usage :** Les algorithmes qui ne font pas d'hypothèses sur la distribution des données (comme les K-Plus Proches Voisins ou les Réseaux de Neurones traitant des images où les pixels vont de 0 à 255).

## 3. La Standardisation (Z-Score Scaling)

La standardisation centre la distribution sur $0$ et met l'écart-type à $1$. Elle transforme les données pour qu'elles ressemblent à une loi normale standard $\mathcal{N}(0, 1)$.

**Formule mathématique :**
$$X_{std} = \frac{X - \mu}{\sigma}$$
*(où $\mu$ est la moyenne et $\sigma$ est l'écart-type de la variable).*

* **Propriétés :** Après transformation, la moyenne de la variable est de $0$ et son écart-type est de $1$. Contrairement au Min-Max, il n'y a pas de bornes strictes (les valeurs peuvent aller au-delà de $[-3, 3]$).

* **Avantage :** Nettement plus robuste aux valeurs aberrantes que la normalisation Min-Max, car la moyenne et l'écart-type sont moins impactés par une valeur extrême que le simple maximum.

* **Cas d'usage :** La majorité des algorithmes de Machine Learning, en particulier la PCA, le SVM, la Régression Logistique et tout modèle régularisé (L1/L2).

## 4. Alternative : Le Robust Scaler

Si le jeu de données contient énormément d'outliers impossibles à supprimer, la moyenne et l'écart-type de la Standardisation classique seront tout de même corrompus. 
On utilise alors le *Robust Scaler*, qui remplace la moyenne par la **médiane**, et l'écart-type par l'**Écart Interquartile (IQR)** (vus dans la Fiche 9) :

$$X_{robust} = \frac{X - \text{Médiane}}{Q_3 - Q_1}$$

![Image_BoxPlot](../00_Images/Pasted%20image%2020260617162656.png)