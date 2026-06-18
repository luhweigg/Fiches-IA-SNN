## 1. Définition et Objectif
La régression est une tâche d'apprentissage supervisé dont l'objectif est de prédire une variable cible continue (un nombre réel) à partir d'une ou plusieurs variables. 
## 2. La Régression Linéaire
### A. Régression Linéaire Simple (1 variable)
Le modèle suppose une relation linéaire stricte entre la feature d'entrée $x$ et la cible $y$. L'équation mathématique est :

$$
y = \beta_0 + \beta_1 x + \epsilon
$$

* $\beta_0$ : L'ordonnée à l'origine (intercept).
* $\beta_1$ : Le coefficient directeur (pente).
* $\epsilon$ : L'erreur ou bruit résiduel.

![Image_Regression_Lineaire](../00_Images/Pasted%20image%2020260611111943.png)

### B. Régression Linéaire Multiple (N variables)
S'il y a $p$ variables explicatives, le modèle devient un hyperplan dans un espace multidimensionnel :

$$
y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_p x_p + \epsilon
$$

**Fonctionnement (Optimisation) :**
L'algorithme cherche à estimer les paramètres $\beta$ optimaux en utilisant la méthode des **Moindres Carrés Ordinaires (MCO)**. L'objectif est de minimiser la somme des carrés des écarts entre les valeurs prédites $\hat{y}$ et les valeurs réelles observées $y$.

![Image_Regression_Lineaire_Multiple](../00_Images/Pasted%20image%2020260611152714.png)

### C. La Descente de Gradient (Gradient Descent)

La méthode exacte des Moindres Carrés (MCO) demande d'inverser des matrices. Si le jeu de données est immense (millions de lignes), l'ordinateur sature. On utilise alors la **Descente de Gradient**, un algorithme d'optimisation itératif.

**Le principe :**
1. On initialise les paramètres (les poids $\beta$) aléatoirement.
2. On définit une **Fonction de Coût** $J(\beta)$ (généralement la MSE) qui mesure l'erreur globale du modèle.
3. On calcule le **gradient** (le vecteur des dérivées partielles) de cette fonction de coût. Ce gradient indique la direction mathématique où l'erreur augmente le plus vite.
4. On met à jour les paramètres en faisant un "pas" dans la direction **opposée** au gradient pour descendre vers le point où l'erreur est minimale :

$$
\beta_j := \beta_j - \alpha \frac{\partial J(\beta)}{\partial \beta_j}
$$

* $\alpha$ (Alpha) est le **Taux d'Apprentissage (Learning Rate)**. C'est un hyperparamètre crucial : 
  * S'il est trop petit, l'algorithme met un temps infini à converger. 
  * S'il est trop grand, l'algorithme "enjambe" le minimum et diverge (l'erreur explose au lieu de se réduire).

![Gradient_Descent](../00_Images/Pasted%20image%2020260611152921.png)

## 3. La Régression Polynomiale
Lorsque les données ne suivent pas une ligne droite, la régression linéaire simple sous-performe (Underfitting). La régression polynomiale modélise des relations non linéaires en créant de nouvelles features qui sont des puissances de la feature initiale :

$$
y = \beta_0 + \beta_1 x + \beta_2 x^2 + \dots + \beta_d x^d + \epsilon
$$

* $d$ : Le degré du polynôme.
* *Attention :* Bien qu'elle modélise une courbe, cette méthode reste un "modèle linéaire" du point de vue de l'estimation des paramètres $\beta$. Augmenter le degré $d$ de façon excessive mène inévitablement à l'Overfitting : le modèle apprendra le bruit statistique au lieu de la tendance.

![Image_Regression_Polynomiale](../00_Images/Pasted%20image%2020260611113530.png)

## 4. Métriques d'Évaluation
Une fois le modèle entraîné, on mesure ses erreurs de prédiction (les résidus : $y_i - \hat{y}_i$). Soit $N$ le nombre total d'observations :

* **MAE (Mean Absolute Error - Erreur Absolue Moyenne) :**
  $$MAE = \frac{1}{N} \sum_{i=1}^{N} |y_i - \hat{y}_i|$$
  La moyenne des erreurs en valeur absolue. Elle est robuste aux valeurs aberrantes (*outliers*) et s'interprète facilement, car elle conserve l'unité de la variable cible.

* **MSE (Mean Squared Error - Erreur Quadratique Moyenne) :**
  $$MSE = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2$$
  La moyenne des erreurs au carré. Ce carré facilite la dérivation mathématique pour l'optimisation, mais il pénalise très fortement les grandes erreurs de prédiction.

![Image_MSE](../00_Images/Pasted%20image%2020260611113638.png)

* **RMSE (Root Mean Squared Error - Racine de l'Erreur Quadratique Moyenne) :**
  $$RMSE = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2}$$
  C'est la racine carrée de la MSE. Son avantage principal est de ramener la métrique d'évaluation dans l'unité de mesure originale de la variable cible, rendant la lecture plus intuitive.

* **$R^2$ (Coefficient de Détermination) :**
  $$R^2 = 1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y})^2}$$
  *où $\bar{y}$ est la moyenne des valeurs réelles.*
  C'est une métrique relative, généralement comprise entre 0 et 1. Un $R^2$ de 0.85 signifie que 85% de la variance de la variable cible est expliquée par le modèle. Un $R^2$ de 1.0 indique une prédiction parfaite.