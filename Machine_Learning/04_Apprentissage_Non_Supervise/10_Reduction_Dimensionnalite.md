## 1. Définition et Objectif

La réduction de dimensionnalité consiste à transformer un jeu de données de haute dimension (avec un grand nombre $d$ de *features*) en un jeu de données de plus faible dimension $k$ (avec $k \ll d$), tout en conservant le maximum d'informations pertinentes.

**Objectifs principaux :**
* **Visualisation :** Permettre de représenter des données complexes en 2D ou 3D.
* **Compression :** Réduire l'espace de stockage et le temps de calcul des algorithmes ultérieurs.
* **Lutter contre le bruit :** Éliminer les variables redondantes (fortement corrélées).

## 2. Le Fléau de la Dimension (The Curse of Dimensionality)

C'est le phénomène mathématique qui rend la réduction de dimension indispensable. 
À mesure que le nombre de dimensions augmente :
1. Le volume de l'espace croît de manière exponentielle. Les données deviennent extrêmement "clairsemées" (éparpillées).
2. La distance entre n'importe quelle paire de points tend à devenir identique. Par conséquent, les métriques géométriques (comme la distance euclidienne) perdent leur sens, ce qui rend les algorithmes basés sur la distance (comme KNN ou K-Means) inopérants.

![Image_Diemnsionality](../00_Images/Pasted%20image%2020260611155309.png)

## 3. Algorithmes Linéaires : PCA (Analyse en Composantes Principales)

La PCA est la technique de réduction de dimension linéaire la plus utilisée.

* **Principe :** Elle cherche à créer de nouvelles variables orthogonales (indépendantes) appelées **Composantes Principales**. Ces composantes sont des combinaisons linéaires des variables d'origine.

* **Objectif mathématique :** Trouver les axes de projection qui maximisent la **variance** des données. La première composante capture le maximum de variance, la deuxième capture le maximum de variance restante tout en étant orthogonale à la première, etc...

* **Fonctionnement sous-jacent :** L'algorithme calcule la matrice de covariance des données centrées-réduites, puis procède à une extraction des valeurs propres  (*eigenvalues*) et des vecteurs propres (*eigenvectors*). Les vecteurs propres associés aux plus grandes valeurs propres forment les axes des composantes principales.

![Image_PCA](../00_Images/Pasted%20image%2020260611155501.png)

## 4. Algorithmes Non Linéaires (Manifold Learning)

Les méthodes linéaires comme la PCA échouent lorsque les données forment des structures géométriques complexes (comme un rouleau suisse). On utilise alors des algorithmes non linéaires, principalement pour la visualisation.

### A. t-SNE (t-Distributed Stochastic Neighbor Embedding)

* **Principe :** Il modélise les similitudes entre les points sous forme de probabilités. Il calcule la probabilité que deux points soient voisins dans l'espace de haute dimension, et cherche à recréer cette même distribution de probabilité dans un espace de faible dimension (2D ou 3D).

* **Objectif mathématique :** Minimiser la divergence de Kullback-Leibler (KL) entre la distribution de probabilité de l'espace d'origine et celle de l'espace réduit.

* **Caractéristique :** Excellent pour séparer visuellement des clusters imbriqués. Cependant, il ne préserve pas bien la structure globale (les distances entre les clusters n'ont pas de sens géographique strict) et il est lourd en temps de calcul.

![Image_t-SNE](../00_Images/Pasted%20image%2020260611155859.png)

### B. UMAP (Uniform Manifold Approximation and Projection)

* **Principe :** Algorithme plus récent basé sur la topologie algébrique. Il construit un graphe représentant la structure locale des données en haute dimension, puis optimise la disposition d'un graphe équivalent en basse dimension.

* **Avantages par rapport à t-SNE :** 1. Plus rapide (passe mieux à l'échelle sur de très grands jeux de données).
  2. Préserve nettement mieux la structure globale (la distance relative entre les différents clusters reste significative).
  3. Peut être utilisé comme étape de prétraitement avant d'entraîner un modèle supervisé (ce que l'on évite généralement avec t-SNE).

![Image_UMAP](../00_Images/Pasted%20image%2020260611155745.png)