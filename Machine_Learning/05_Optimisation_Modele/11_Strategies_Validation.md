## 1. Les Limites du Simple Découpage (Hold-out)

Le découpage basique Train/Validation/Test vu dans la Fiche 3 présente une faille statistique : si le jeu de données est de taille modeste, la répartition aléatoire peut placer des exemples atypiques dans le set de validation. L'évaluation de la performance devient dépendante du hasard du tirage initial (forte variance de l'évaluation).

## 2. La Validation Croisée à K blocs (K-Fold Cross-Validation)

Pour obtenir une estimation robuste et non biaisée de la capacité de généralisation d'un modèle, on utilise la validation croisée.

**Le principe opératoire :**
1. Le jeu de données d'entraînement complet est divisé en $K$ sous-ensembles mutuellement exclusifs (les *folds*) de taille égale.
2. Le processus d'entraînement est répété $K$ fois de manière indépendante.
3. À chaque itération $i$, le modèle est entraîné sur $K-1$ blocs agrégés, et évalué sur le bloc $i$ restant (qui fait office de set de validation).
4. La performance finale du modèle est la moyenne arithmétique des $K$ scores de validation obtenus.

$$CV_{score} = \frac{1}{K} \sum_{i=1}^{K} Score_i$$

**Avantage mathématique :** Chaque observation du jeu de données original a été utilisée exactement une fois comme donnée de validation et $K-1$ fois comme donnée d'entraînement.

![Image_Cross_validation](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611162443.png)

## 3. La Stratification (Stratified K-Fold)

**Le problème :** Dans le cadre d'un problème de classification avec des classes déséquilibrées (ex: 95% de classe 0, 5% de classe 1), un découpage aléatoire ou un K-Fold classique pourrait générer un bloc de validation ne contenant *aucune* observation de la classe minoritaire, rendant le calcul des métriques (Precision, Recall) mathématiquement impossible ou faussé.

**La solution :** La stratification est une contrainte algorithmique appliquée lors du partitionnement. Elle force le maintien rigoureux de la distribution statistique initiale des classes de la variable cible ($Y$) au sein de chaque bloc généré.

![Image_Stratifield_Kfold](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611162609.png)

## 4. Cas Particuliers de Validation

* **Leave-One-Out Cross-Validation (LOOCV) :** C'est le cas extrême du K-Fold où $K = N$ ($N$ étant le nombre total d'observations). Le modèle s'entraîne sur $N-1$ données et est validé sur la donnée unique restante. Le coût de calcul est colossal, cette méthode est strictement réservée aux très petits jeux de données.

![Image_LOOCV](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611162646.png)

* **Validation Temporelle (Time Series Split) :** Si les données comportent une dimension temporelle stricte (ex: prévisions boursières), le K-Fold aléatoire est interdit car il entraînerait une fuite d'information du futur vers le passé (*Data Leakage*). On utilise un découpage séquentiel progressif : le modèle est entraîné sur une fenêtre temporelle passée pour valider sur la fenêtre temporelle immédiatement future.

