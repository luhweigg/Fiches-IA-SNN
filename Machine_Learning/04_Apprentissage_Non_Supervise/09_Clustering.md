## 1. Définition et Objectif

Le clustering (regroupement) est une tâche d'apprentissage non supervisé. Les données d'entrée $X$ ne possèdent aucune étiquette $Y$.
L'objectif est de diviser le jeu de données en sous-groupes (clusters) distincts de sorte que :
1. **La variance intra-cluster soit minimisée :** Les points d'un même cluster se ressemblent fortement.
2. **La variance inter-cluster soit maximisée :** Les clusters sont très différents les uns des autres.

## 2. K-Means (K-Moyennes)

C'est un algorithme basé sur la notion de centroïdes (centres géométriques).

* **Principe :** Il partitionne les données en $K$ clusters, où $K$ est un hyperparamètre défini à l'avance par l'utilisateur.
* **Fonctionnement itératif :**
  1. Initialisation aléatoire de $K$ centroïdes $\mu_1, \dots, \mu_K$.
  2. **Assignation :** Chaque point $x_i$ est assigné au cluster dont le centroïde est le plus proche (généralement via la distance euclidienne).
  3. **Mise à jour :** La position de chaque centroïde est recalculée en prenant la moyenne mathématique de tous les points qui lui ont été assignés.
  4. Répéter les étapes 2 et 3 jusqu'à convergence (les centroïdes ne bougent plus).

* **Objectif mathématique :** Minimiser l'Inertie (la somme des carrés des distances intra-cluster) :
  $$Inertia = \sum_{j=1}^{K} \sum_{x_i \in C_j} ||x_i - \mu_j||^2$$
* **Limites :** Suppose que les clusters sont sphériques et de taille similaire. Très sensible aux valeurs aberrantes (*outliers*).

![[Pasted image 20260611153411.png]]

## 3. DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

C'est un algorithme basé sur la densité. Il est capable d'identifier des clusters de formes arbitraires et de repérer les données aberrantes.

* **Hyperparamètres :**
  * $\epsilon$ (Epsilon) : Le rayon de voisinage autour d'un point.
  * $MinPts$ : Le nombre minimum de points requis dans le rayon $\epsilon$ pour former une région dense.

* **Typologie des points :**
  * **Point cœur (Core) :** Possède au moins $MinPts$ points dans son voisinage $\epsilon$.
  * **Point frontière (Border) :** N'a pas $MinPts$ dans son voisinage, mais se trouve dans le voisinage d'un point cœur.
  * **Point de bruit (Noise/Outlier) :** N'est ni cœur, ni frontière.

* **Avantages :** Il n'est pas nécessaire de spécifier le nombre de clusters à l'avance. Il isole nativement le bruit.

![[Pasted image 20260611153914.png]]

## 4. CAH (Classification Ascendante Hiérarchique)

C'est un algorithme basé sur la connectivité qui construit une hiérarchie de clusters sous la forme d'un arbre appelé **Dendrogramme**.

* **Principe (Approche agglomérative) :**
  1. Au départ, chaque point de donnée est considéré comme son propre cluster (soit $N$ clusters).
  2. À chaque itération, l'algorithme fusionne les deux clusters les plus proches en un seul.
  3. Le processus se répète jusqu'à ce qu'il ne reste plus qu'un unique cluster contenant toutes les données.

* **Critères de liaison (Linkage) :** Pour calculer la distance entre deux clusters (composés de plusieurs points), plusieurs méthodes existent :
  * *Single Linkage :* Distance entre les deux points les plus proches des deux clusters.
  * *Complete Linkage :* Distance entre les deux points les plus éloignés.
  * *Average Linkage :* Moyenne des distances entre tous les points.
  * *Méthode de Ward :* Fusionne les clusters qui minimisent l'augmentation de la variance intra-cluster globale.

* **Utilisation :** On "coupe" le dendrogramme à la hauteur souhaitée pour obtenir le nombre $K$ de clusters désiré.

![[Pasted image 20260611154108.png]]

## 5. Évaluation et Choix du nombre de Clusters

En apprentissage non supervisé, l'absence de variable cible (*Target*) nécessite des métriques purement géométriques.

### A. La Méthode du Coude (Elbow Method)

Principalement utilisée pour K-Means afin de choisir le meilleur hyperparamètre $K$.
* **Principe :** On entraîne le modèle pour différentes valeurs de $K$ (ex: de 1 à 10) et on trace l'Inertie (la variance intra-cluster) sur un graphique. Plus on ajoute de clusters, plus l'inertie baisse.
* **La règle :** On cherche le point d'inflexion (le "coude") sur la courbe. C'est le moment où l'ajout d'un cluster supplémentaire n'apporte plus de réduction significative de la variance.

### B. Le Score de Silhouette
Il évalue la qualité globale d'un clustering. Pour chaque observation, il calcule à quel point elle est proche des autres points de son cluster (cohésion) par rapport aux points du cluster voisin le plus proche (séparation).
* Le score est compris entre -1 et 1. 
* Proche de **1** : Le point est parfaitement assigné et bien isolé des autres clusters.
* Proche de **0** : Le point est situé exactement à la frontière entre deux clusters.
* Proche de **-1** : Le point a été assigné au mauvais cluster.