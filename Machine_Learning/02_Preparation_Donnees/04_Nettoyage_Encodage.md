## 1. Gestion des Valeurs Manquantes (Imputation)

Les algorithmes mathématiques (à de rares exceptions près comme XGBoost) ne peuvent pas traiter des matrices contenant des valeurs nulles ou indéfinies (`NaN`).

* **Suppression (Deletion) :**
  * *Lignes :* Si le pourcentage de données manquantes sur une ligne est très élevé, ou si le dataset est gigantesque.
  * *Colonnes :* Si une *feature* possède plus d'un certain seuil (ex: 60%) de valeurs manquantes, elle est souvent supprimée car le manque d'information la rend non prédictive.

* **Imputation Statistique :**
  Remplacement de la valeur manquante par une constante calculée sur le reste de la distribution.
  * *Moyenne (Mean) :* Sensible aux valeurs aberrantes.
  * *Médiane (Median) :* Robuste aux valeurs aberrantes. Privilégiée pour les distributions asymétriques.
  * *Mode :* La valeur la plus fréquente. Utilisée exclusivement pour les variables catégorielles.

* **Imputation Algorithmique (ex: KNN Imputer) :**
  Le modèle cherche les $K$ voisins les plus proches de l'observation incomplète (en calculant la distance sur les autres *features* disponibles) et attribue la valeur moyenne de ces voisins à la donnée manquante.

![[Pasted image 20260611161011.png]]

## 2. Gestion des Valeurs Aberrantes (Outliers)

Un *outlier* est une observation qui dévie de manière si significative des autres observations qu'elle éveille le soupçon d'avoir été générée par un mécanisme différent (ex: erreur de capteur).

![[Pasted image 20260611161012.png]]

### A. Méthodes de Détection

* **Le Z-Score :**
  Mesure la distance d'un point par rapport à la moyenne en termes d'écarts-types. Un seuil absolu (souvent 3) est fixé.
  $$Z = \frac{x_i - \mu}{\sigma}$$
  *Règle :* Si $|Z| > 3$, la valeur est considérée comme aberrante. Valide uniquement si la distribution sous-jacente est proche d'une loi normale.

* **L'Écart Interquartile (IQR - Interquartile Range) :**
  Méthode non paramétrique, robuste à la distribution.
  $$IQR = Q_3 - Q_1$$
  *Règle :* Les bornes d'acceptation sont définies par $[Q_1 - 1.5 \times IQR \ ; \ Q_3 + 1.5 \times IQR]$. Tout point en dehors de cet intervalle est un *outlier*.

### B. Traitement

* **Suppression :** Si l'on prouve qu'il s'agit d'une erreur de saisie.

* **Capping / Clipping :** Plafonner les valeurs extrêmes aux bornes définies (ex: ramener tout ce qui dépasse le 99e centile à la valeur du 99e centile).

* **Transformation Mathématique :** Appliquer une fonction (comme le logarithme $x \mapsto \log(x)$) pour écraser l'échelle de la distribution et rapprocher les valeurs extrêmes de la masse principale.

## 3. Transformation du Texte en Nombres (Encodage)

Les modèles d'apprentissage automatique opèrent sur des espaces vectoriels réels ($\mathbb{R}^d$). Toute variable catégorielle (texte) doit être projetée dans cet espace.

### A. Label Encoding (Encodage Ordinal)

Chaque catégorie unique est remplacée par un entier.
* *Mécanisme :* $[\text{"Faible"}, \text{"Moyen"}, \text{"Fort"}] \rightarrow [0, 1, 2]$

* *Cas d'usage :* Strictement réservé aux variables **ordinales** (où il existe une relation d'ordre inhérente).

* *Le danger :* Si appliqué à des variables nominales (sans ordre, ex: des couleurs), l'algorithme interprétera faussement que $2 > 1 > 0$, créant une relation mathématique inexistante (ex: "Rouge" serait supérieur à "Bleu").

### B. One-Hot Encoding (Encodage Nominal)

Transforme une variable catégorielle comportant $C$ modalités en $C$ nouvelles colonnes binaires (0 ou 1) mutuellement exclusives.

* *Mécanisme :* La variable "Couleur" avec les modalités $[\text{"Rouge"}, \text{"Vert"}, \text{"Bleu"}]$ devient 3 variables :
  * $Couleur\_Rouge \rightarrow [1, 0, 0]$
  * $Couleur\_Vert \rightarrow [0, 1, 0]$
  * $Couleur\_Bleu \rightarrow [0, 0, 1]$
  
* *Cas d'usage :* Pour les variables **nominales**.

* *Conséquence (Le piège de la colinéarité) :* La somme des $C$ colonnes vaut toujours 1. Cela introduit une multicolinéarité parfaite, perturbant certains modèles (comme la régression linéaire). On applique souvent le *Drop First* (supprimer une des $C$ colonnes, créant $C-1$ colonnes) pour résoudre ce problème géométrique.