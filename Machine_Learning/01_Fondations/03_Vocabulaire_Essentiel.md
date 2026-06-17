## 1. Features et Target (Les Entrées et la Sortie)

* **Features (Variables explicatives / X) :** Ce sont les variables indépendantes, les attributs mesurables fournis en entrée à l'algorithme. Dans un jeu de données tabulaire, elles correspondent aux colonnes utilisées pour effectuer la prédiction.
    *Exemple :* Surface en m², nombre de pièces, code postal.

* **Target (Variable cible / Y) :** C'est la variable dépendante que le modèle doit apprendre à prédire. Elle n'est présente que dans le cadre d'un apprentissage supervisé.
    *Exemple :* Le prix final de l'appartement.

## 2. Le Découpage des Données (Train / Validation / Test Split)

Le jeu de données initial doit être partitionné pour évaluer objectivement les performances du modèle.

* **Train Set (Jeu d'entraînement - env. 70-80%) :** Le sous-ensemble de données utilisé exclusivement pour ajuster les paramètres internes de l'algorithme lors de la phase d'apprentissage.

* **Validation Set (Jeu de validation - env. 10-15%) :** Le sous-ensemble utilisé pour évaluer le modèle en cours d'entraînement. Il sert à comparer différents modèles et à optimiser les hyperparamètres sans fausser l'évaluation finale.

* **Test Set (Jeu de test - env. 10-15%) :** Le sous-ensemble strictement isolé tout au long du développement. Il est utilisé une unique fois à la fin du processus pour obtenir une mesure non biaisée de la capacité de généralisation du modèle sur de nouvelles données.

## 3. Overfitting et Underfitting

L'objectif d'un modèle est la **généralisation** : sa capacité à produire des prédictions précises sur des données qu'il n'a jamais rencontrées.

* **Underfitting (Sous-apprentissage) :** Le modèle est trop simple pour capturer les relations mathématiques sous-jacentes dans les données. 
    * *Symptôme :* Les erreurs de prédiction sont élevées à la fois sur le Train set et le Validation/Test set.

* **Overfitting (Surapprentissage) :** Le modèle est trop complexe. Il ne se contente pas d'apprendre la tendance générale, mais modélise également le "bruit" statistique ou les anomalies spécifiques aux données d'entraînement.
    * *Symptôme :* L'erreur est proche de zéro sur le *Train set*, mais explose sur le *Validation/Test set* (incapacité à généraliser).

**Solution :** 
**Dropout (Désactivation aléatoire)** Technique pour éviter le surapprentissage (Overfitting). À chaque itération, on "éteint" temporairement un pourcentage aléatoire de neurones. Cela force le réseau à distribuer l'information et empêche les neurones de trop dépendre les uns des autres.


## 4. Le Compromis Biais / Variance (Bias-Variance Tradeoff)

L'erreur de prédiction d'un modèle se décompose mathématiquement en trois parties : le Biais, la Variance et le bruit irréductible.

* **Biais (Bias) :** L'erreur introduite en approximant un problème réel complexe par un modèle trop restrictif ou linéaire. Un biais élevé empêche l'apprentissage correct et mène directement à l'Underfitting.

* **Variance :** La sensibilité de l'algorithme aux fluctuations du jeu de données d'entraînement. Une variance élevée signifie que le modèle modifierait radicalement ses prédictions si on l'entraînait sur un échantillon légèrement différent. Elle mène directement à l'Overfitting.

* **Le Compromis :** L'optimisation en Machine Learning consiste à minimiser l'erreur totale. Diminuer la complexité du modèle réduit la variance mais augmente le biais, et inversement. Le point d'équilibre correspond à la meilleure généralisation possible.