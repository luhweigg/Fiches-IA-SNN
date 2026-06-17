## 1. Collecte des Données (Data Collection)

* **Le principe :** Récupérer la matière première nécessaire à l'apprentissage.

* **Les sources :** Bases de données, fichiers (CSV, JSON), APIs, ou extraction web (Scraping).

* **Le point d'attention :** La qualité et la quantité des données définissent le plafond de performance du futur modèle.

## 2. Analyse Exploratoire (EDA - Exploratory Data Analysis)

* **Le principe :** Inspecter visuellement et statistiquement les données avant de les manipuler.

* **L'objectif :**
	* Comprendre la distribution des variables.
    * Identifier les corrélations entre les différentes données.
    * Repérer les anomalies (outliers).

## 3. Préparation et Nettoyage (Data Preprocessing & Feature Engineering)

* **Le principe :** Transformer les données brutes dans un format mathématique strict, exploitable par un algorithme. C'est souvent l'étape la plus chronophage.

* **Les actions clés :**
	
    * **Nettoyage :** Gérer les valeurs manquantes (les supprimer ou les remplacer par une moyenne/médiane).
	
    * **Encodage :** Convertir les données textuelles ou catégoriques en valeurs numériques (ex: Rouge=1, Vert=2, Bleu=3).
    
    * **Mise à l'échelle (Scaling) :** Ramener toutes les valeurs sur une échelle commune pour éviter qu'une variable n'écrase les autres à cause de sa grandeur (ex: âge vs salaire).
    
    * **Feature Engineering :** Créer de nouvelles variables pertinentes à partir de celles existantes (ex: créer une colonne "surface_par_piece" à partir des colonnes "surface_totale" et "nombre_pieces").

## 4. Modélisation et Entraînement (Modeling)

* **Le principe :** Choisir l'algorithme adapté au problème (Régression, Classification...) et l'appliquer aux données préparées.

* **La règle d'or :** Ne jamais s'entraîner sur 100% des données.
    * **Train set (70-80%) :** Les données utilisées pour entraîner le modèle.
    * **Validation Set (10-15%) :** Sert à ajuster les hyperparamètres et à comparer différents modèles en cours de développement.
    * **Test set (10-15%) :** Les données mises de côté pour l'évaluation finale.

## 5. Évaluation et Optimisation (Evaluation & Tuning)

* **Le principe :** Mesurer les performances du modèle sur le Test set

* **L'objectif :** 
	* Vérifier la capacité de généralisation du modèle (s'assurer qu'il n'a pas simplement appris par cœur les données d'entraînement (Overfitting).
    * **Optimisation :** Ajuster les paramètres de l'algorithme (hyperparamètres) pour maximiser les métriques de succès (précision, marge d'erreur, etc...).

## 6. Déploiement et Suivi (Deployment & Monitoring)

* **Le principe :** Mettre le modèle en production pour qu'il génère des prédictions sur des données réelles, en temps réel.

* **L'intégration :** Via une API (ex: une boutique en ligne envoie un panier à l'API, le modèle renvoie une recommandation d'achat).

* **Le suivi (Monitoring) :** Les données du monde réel évoluent avec le temps (phénomène appelé *Data Drift*). Il faut surveiller la précision du modèle en continu et le réentraîner périodiquement avec de nouvelles données fraîches.