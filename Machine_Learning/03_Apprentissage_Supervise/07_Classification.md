# Fiche 5 : Apprentissage Supervisé - La Classification

## 1. Définition et Objectif

La classification est une tâche d'apprentissage supervisé où la variable cible $Y$ est **discrète** (catégorielle). L'objectif est d'affecter une observation à une classe prédéfinie.
* **Classification binaire :** Deux classes possibles (ex: 0 ou 1, Spam ou Non-Spam).
* **Classification multiclasse :** Plus de deux classes (ex: Chien, Chat, Oiseau).

## 2. Algorithmes Principaux

### A. Régression Logistique
Malgré son nom, c'est un algorithme de classification binaire. Il modélise la probabilité qu'une observation appartienne à la classe par défaut (ex: classe 1).
Il utilise la fonction logistique (ou sigmoïde) pour transformer la combinaison linéaire des features en une probabilité comprise entre 0 et 1 :

$$
p(X) = \frac{1}{1 + e^{-(\beta_0 + \beta_1 X_1 + \dots + \beta_p X_p)}}
$$

* Une règle de décision est ensuite appliquée : si $p(X) \geq 0.5$, la classe prédite est 1, sinon 0.

**Fonction de Coût (Log-Loss / Binary Cross-Entropy) :**
Pour trouver les paramètres optimaux via la descente de gradient, la régression logistique ne minimise pas la MSE, mais la Log-Loss. Cette fonction pénalise de manière logarithmique (très fortement) les prédictions qui sont à la fois très confiantes et fausses :

$$
L(y, p) = - [y \log(p) + (1 - y) \log(1 - p)]
$$

* $y$ : La vraie classe (0 ou 1).
* $p$ : La probabilité prédite par le modèle.

### B. SVM (Support Vector Machines - Machines à Vecteurs de Support)

Le SVM cherche à trouver un **hyperplan séparateur** qui divise l'espace des données pour isoler les classes. L'objectif mathématique est de maximiser la marge, c'est-à-dire la distance entre cet hyperplan et les points d'entraînement les plus proches de chaque classe (appelés "vecteurs de support").

![Image_SVM](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611150652.png)

- **L'astuce du noyau (Kernel Trick) :** Si les données ne sont pas linéairement séparables, le SVM utilise une fonction mathématique (le noyau) pour projeter les données dans un espace de dimension supérieure où une séparation linéaire devient possible.

### C. K-NN (K-Nearest Neighbors - K-Plus Proches Voisins)

C'est un algorithme non paramétrique (il n'y a pas d'équation mathématique à optimiser lors de l'entraînement).
Pour prédire la classe d'un nouveau point :
1. Il calcule la distance (généralement la distance euclidienne) entre ce point et tous les autres points du jeu d'entraînement.
2. Il identifie les $K$ points les plus proches (les voisins).
3. Il assigne la classe majoritaire parmi ces $K$ voisins au nouveau point.

![Image_K-NN](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611150556.png)

## 3. Métriques d'Évaluation

Pour évaluer un modèle de classification, on confronte les prédictions aux valeurs réelles via une **Matrice de Confusion**.

Dans un cas binaire, elle contient 4 valeurs :
* **TP (Vrais Positifs) :** Le modèle a prédit Positif, et c'est vrai.
* **TN (Vrais Négatifs) :** Le modèle a prédit Négatif, et c'est vrai.
* **FP (Faux Positifs - Erreur de Type I) :** Le modèle a prédit Positif, mais c'est faux.
* **FN (Faux Négatifs - Erreur de Type II) :** Le modèle a prédit Négatif, mais c'est faux.

À partir de cette matrice, on calcule les métriques suivantes :

* **Accuracy (Exactitude) :** Le ratio de prédictions correctes sur le total.
  $$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$
  *Limite : Trompeur si les classes sont déséquilibrées (ex: 99% de TN).*

* **Precision (Précision) :** Parmi toutes les prédictions positives, combien sont réellement positives ?
  $$Precision = \frac{TP}{TP + FP}$$

* **Recall (Rappel / Sensibilité) :** Parmi tous les cas réellement positifs, combien le modèle en a-t-il détecté ?
  $$Recall = \frac{TP}{TP + FN}$$

* **F1-Score :** La moyenne harmonique entre la Précision et le Rappel. Utilisé pour trouver un compromis lorsque les faux positifs et les faux négatifs ont un coût important.
  $$F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}$$

* **Courbe ROC (Receiver Operating Characteristic) :** C'est un graphique qui évalue les performances d'un classifieur binaire en faisant varier le seuil de décision (de 0 à 1). Il trace le taux de Vrais Positifs (Recall) en ordonnée en fonction du taux de Faux Positifs en abscisse.

* **AUC (Area Under the Curve) :** C'est l'aire sous la courbe ROC. 
  * AUC = 0.5 : Le modèle répond au hasard (ligne diagonale).
  * AUC = 1.0 : Le modèle prédit parfaitement.
  * *L'avantage :* C'est la métrique reine pour comparer deux modèles entre eux de manière globale, indépendamment du seuil choisi.