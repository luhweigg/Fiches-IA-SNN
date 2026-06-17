## 1. Paramètres vs Hyperparamètres

Avant d'optimiser un algorithme, la distinction mathématique entre ces deux éléments est stricte :
* **Paramètres (Poids / $\theta$) :** Variables internes du modèle apprises automatiquement lors de la phase d'entraînement en minimisant la fonction de coût (ex: les coefficients $\beta$ d'une régression linéaire).

* **Hyperparamètres :** Variables de configuration de l'algorithme, définies *avant* l'entraînement. L'algorithme ne peut pas les apprendre par lui-même (ex: le taux d'apprentissage $\alpha$, le nombre d'arbres dans une Random Forest, ou la profondeur maximale d'un arbre).

L'optimisation du modèle consiste à trouver la combinaison d'hyperparamètres qui minimise l'erreur sur le jeu de validation.

## 2. Recherche d'Hyperparamètres (Tuning)

### A. Grid Search (Recherche par grille)

* **Principe :** C'est un algorithme de force brute. L'utilisateur définit un dictionnaire de valeurs possibles pour chaque hyperparamètre. L'algorithme évalue le modèle (souvent couplé à une validation croisée) sur toutes les combinaisons du produit cartésien de ces listes.

* **Complexité :** Le coût de calcul croît de manière exponentielle $O(N^d)$ avec le nombre d'hyperparamètres $d$.

* **Cas d'usage :** Espace de recherche de faible dimension (1 à 3 hyperparamètres maximum).

![[Pasted image 20260611163020.png]]

### B. Random Search (Recherche stochastique)

* **Principe :** Au lieu d'explorer une grille exhaustive, l'algorithme tire un nombre $N$ défini de combinaisons de manière aléatoire (ou selon une distribution statistique spécifique, comme une loi uniforme ou log-uniforme) dans l'espace des hyperparamètres.

* **Avantage mathématique :** Il est prouvé que Random Search est statistiquement plus efficace que Grid Search. Dans une grille, la majorité du temps de calcul est perdu à faire varier des hyperparamètres qui n'ont pas d'impact significatif sur la fonction de coût, pendant que les hyperparamètres critiques sont sous-explorés. Le tirage aléatoire garantit une meilleure couverture marginale des dimensions importantes.

## 3. La Régularisation : Concept Général

La régularisation est une technique permettant de combattre l'Overfitting (variance élevée) en pénalisant la complexité mathématique du modèle. 
On modifie la fonction de coût $L(\theta)$ en y ajoutant un terme de pénalité contrôlé par un hyperparamètre $\lambda$ (Lambda, ou $\alpha$ dans certaines documentations).

**Équation générale :**
$$Co\hat{u}t_{r\acute{e}gularis\acute{e}} = L(\theta) + \lambda \times P\acute{e}nalit\acute{e}$$

* Si $\lambda = 0$ : Aucune régularisation (risque d'Overfitting).
* Si $\lambda \rightarrow \infty$ : Pénalité maximale, les poids $\theta$ tendent vers 0 (risque d'Underfitting massif).

## 4. Régularisation L2 (Ridge) : Réduction d'amplitude

La pénalité L2 est la somme des carrés des coefficients du modèle.

$$L_{Ridge}(\theta) = L(\theta) + \lambda \sum_{j=1}^{p} \theta_j^2$$

* **Comportement mathématique :** La dérivée de la pénalité L2 (qui vaut $2\theta_j$) diminue à mesure que $\theta_j$ se rapproche de zéro. Par conséquent, la régularisation L2 pousse les coefficients vers zéro de manière asymptotique, sans jamais les annuler strictement.

* **Impact sur les features :** Si deux variables sont fortement corrélées (multicolinéarité), Ridge va répartir le poids de manière égale entre elles, réduisant ainsi la variance globale du modèle et améliorant sa stabilité.

## 5. Régularisation L1 (Lasso) : Sélection de variables

La pénalité L1 est la somme des valeurs absolues des coefficients.

$$L_{Lasso}(\theta) = L(\theta) + \lambda \sum_{j=1}^{p} |\theta_j|$$

* **Comportement mathématique :** Contrairement au carré, la dérivée de la valeur absolue est constante (elle vaut 1 ou -1) quel que soit le point (hors 0). La force de pénalisation ne faiblit pas quand le coefficient diminue. Par conséquent, pour des valeurs suffisantes de $\lambda$, le Lasso force mathématiquement certains coefficients $\theta_j$ à devenir exactement égaux à zéro.

* **Impact sur les features :** Le modèle réalise une sélection de variables (Feature Selection) native. Il annule le poids des features inutiles ou redondantes, produisant un modèle dit "clairsemé" (*sparse*), plus simple à interpréter.