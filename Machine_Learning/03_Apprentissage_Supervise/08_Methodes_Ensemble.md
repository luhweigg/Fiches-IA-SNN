# Fiche 6 : Apprentissage Supervisé - Les Méthodes d'Ensemble

## 1. Définition et Objectif

Les méthodes d'ensemble reposent sur un principe mathématique et statistique simple : la combinaison de plusieurs modèles prédictifs simples (appelés weak learners) permet de construire un modèle final beaucoup plus performant et robuste (un strong learner).

L'objectif principal est de réduire l'erreur de généralisation en jouant sur le compromis Biais/Variance abordé dans la fiche [[03_Vocabulaire_Essentiel]].

## 2. Le Modèle de Base : L'Arbre de Décision (Decision Tree)

La grande majorité des méthodes d'ensemble utilisent l'arbre de décision comme modèle de base.

**Fonctionnement :** Il partitionne l'espace des données de manière récursive en posant des questions binaires sur les *features* (ex: "La surface est-elle > 50m² ?").

**Critère de division (Classification) :** L'algorithme choisit la coupure qui maximise la pureté des nœuds enfants. On utilise généralement l'Indice de Gini ou l'Entropie de Shannon.

Formule de l'Impureté de Gini pour un nœud contenant des proportions $p_i$ de $C$ classes :

$$
Gini = 1 - \sum_{i=1}^{C} p_i^2
$$

* **Le problème de l'arbre seul :** Un arbre profond qui se développe jusqu'à ce que chaque feuille soit pure a une variance extrêmement élevée (Overfitting sévère). Il mémorise les données.

## 3. Le Bagging (Bootstrap Aggregating) et Random Forest

Le Bagging est une méthode de création d'ensemble fonctionnant en parallèle dans le but de réduire la variance sans augmenter le biais.

### A. Le Principe du Bagging
1. **Bootstrap :** On crée $B$ nouveaux jeux d'entraînement en effectuant des tirages aléatoires avec remise à partir du jeu de données original.
2. **Entraînement :** On entraîne un modèle (ex: un arbre) indépendamment sur chaque sous-échantillon.
3. **Agrégation :** La prédiction finale est obtenue par :
   * Vote majoritaire pour la classification.
   * Moyenne arithmétique pour la régression : $\hat{y} = \frac{1}{B} \sum_{b=1}^{B} \hat{f}_b(x)$

### B. Le Random Forest (Forêt Aléatoire)
C'est une amélioration du Bagging appliqué aux arbres de décision. Pour éviter que tous les arbres ne se ressemblent trop (s'ils utilisent tous la même *feature* dominante), on force une décorrélation.
* **Le mécanisme :** À chaque division d'un nœud dans un arbre, l'algorithme ne peut choisir la meilleure coupure que parmi un sous-ensemble aléatoire de *features* (généralement $\sqrt{p}$ où $p$ est le nombre total de features).

## 4. Le Boosting et Gradient Boosting

Le Boosting est une méthode de création d'ensemble fonctionnant de manière séquentielle dans le but de réduire le biais (et la variance dans une moindre mesure).

### A. Le Principe du Boosting
Les modèles sont entraînés les uns après les autres. Chaque nouveau modèle se concentre spécifiquement sur les erreurs (les observations mal prédites) du modèle précédent pour les corriger.

### B. Gradient Boosting
Il formalise le Boosting comme un problème d'optimisation numérique. Chaque nouvel arbre est entraîné pour prédire le **pseudo-résidu** (l'erreur) du modèle global précédent, en calculant le gradient de la fonction de perte $L$ :

$$
r_{im} = - \left[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \right]_{F=F_{m-1}}
$$

*où $F_{m-1}$ est le modèle d'ensemble à l'étape précédente.*
Le modèle final $F_M(x)$ est la somme pondérée de tous les arbres construits.

### C. XGBoost (Extreme Gradient Boosting)
C'est l'implémentation la plus célèbre et performante du Gradient Boosting. 
* **Optimisations :** Il intègre des termes de **régularisation (L1 et L2)** directement dans l'équation de l'objectif pour pénaliser la complexité des arbres. Il gère nativement les valeurs manquantes et parallélise la construction des branches de l'arbre au niveau matériel, le rendant extrêmement rapide et difficile à battre sur des données tabulaires.