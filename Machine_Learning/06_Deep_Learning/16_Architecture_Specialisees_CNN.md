## 1. Les limites du Réseau Dense (MLP) sur les images

Pour traiter une image avec un réseau classique (Fiche [[14_Architectures_Propogation]]), il faut aplatir la grille de pixels 2D en un vecteur 1D (couche *Flatten*).
Cela pose deux problèmes mathématiques et structurels majeurs :

1. **Perte de la topologie spatiale :** L'opération d'aplatissement détruit l'information géométrique. Le réseau ne "sait" plus qu'un pixel est situé juste au-dessus ou à gauche d'un autre.

2. **L'explosion combinatoire des poids :** Une image couleur standard (ex: 1000x1000 pixels $\times$ 3 canaux RGB) aplatie donne un vecteur de 3 millions d'entrées. Connecter ce vecteur à une première couche cachée de seulement 1000 neurones nécessiterait **3 milliards de poids** ($\mathbf{W}$) à apprendre. C'est incalculable et cela mène tout droit à l'Overfitting massif.

Les **CNN (Convolutional Neural Networks)** résolvent ce problème en conservant la structure 2D/3D des données et en utilisant des connexions locales.

## 2. L'Opération de Convolution (Feature Extraction)

Le cœur du CNN est la couche de convolution. Au lieu de connecter chaque neurone à toute l'image, on utilise un petit filtre matriciel (le **Kernel** ou Noyau), généralement de taille 3x3 ou 5x5.

* **Le Principe :** Le filtre glisse (se translate) sur toute la surface de l'image. À chaque position, il effectue un produit scalaire (multiplication élément par élément puis somme) entre ses propres poids et les pixels de l'image qu'il recouvre.

* **Le Résultat :** L'ensemble de ces calculs forme une nouvelle matrice appelée **Feature Map** (Carte de caractéristiques).

* **Partage des Poids (Weight Sharing) :** C'est le coup de génie mathématique du CNN. Un filtre 3x3 ne possède que 9 poids (+ 1 biais). Ce sont **ces mêmes 9 poids** qui glissent sur toute l'image. 
    * *L'avantage :* Si un filtre apprend à détecter un motif (ex: un œil) en haut à gauche, il sera capable de le détecter n'importe où ailleurs sur l'image. C'est l'**invariance par translation**.

![[Pasted image 20260612142244.png]]

## 3. Les Hyperparamètres Géométriques

Pour contrôler le comportement de ce filtre glissant, on définit deux variables avant l'entraînement :

* **Le Pas (Stride) :** Détermine de combien de pixels le filtre se décale à chaque étape. 
  * Un *Stride* de 1 produit une *Feature Map* de taille similaire à l'entrée.
  * Un *Stride* de 2 divise la taille spatiale par deux (le filtre saute un pixel sur deux).7

* **Le Remplissage (Padding) :** Le filtre ne peut pas s'appliquer correctement sur les bords extrêmes (il déborderait de l'image). Par défaut, cela rétrécit l'image en sortie. Pour éviter cette perte spatiale, on applique un *Padding* (souvent le *Zero-Padding*) : on ajoute un cadre de pixels valant "0" tout autour de l'image d'entrée.

## 4. Le Sous-échantillonnage (Pooling Layer)

Intercalée entre les couches de convolution, la couche de *Pooling* a pour but de réduire drastiquement la dimensionnalité spatiale (la largeur et la hauteur) tout en conservant l'information critique.

* **Max Pooling :** La méthode la plus courante. On fait glisser une petite fenêtre (ex: 2x2 avec un *Stride* de 2) sur la *Feature Map* et on ne conserve que la valeur mathématique **maximale** de cette fenêtre.

* **Average Pooling :** Au lieu du maximum, on calcule la moyenne mathématique des valeurs dans la fenêtre. C'est utilisé pour "lisser" les caractéristiques d'une image, bien que le Max Pooling reste le standard industriel.

* **Objectifs :** 
	
	1. Réduire le temps de calcul.
	
	2. Rendre la détection encore plus robuste aux petites déformations spatiales (si un motif bouge d'un pixel, le maximum dans la fenêtre de Pooling restera probablement le même).

![[Pasted image 20260612143222.png]]

## 5. L'Architecture Globale (L'Entonnoir)

Un CNN se divise toujours en deux blocs distincts :

1. **L'extracteur de Features (Le corps) :** Une succession de blocs
   `[Convolution -> ReLU -> Pooling]`. Au fil des couches, l'image devient de plus en plus petite spatialement, mais de plus en plus profonde (le réseau utilise de plus en plus de filtres différents). Les premiers filtres détectent des lignes, les derniers détectent des concepts complexes (des visages, des roues).

2. **Le Classifieur (La tête) :** À la toute fin, les *Feature Maps* très denses sont aplaties (couche `Flatten`) pour former un vecteur 1D. Ce vecteur alimente un réseau dense classique (MLP) qui se termine par une fonction Softmax pour émettre la prédiction finale (ex: Chien, Cheval ou Zèbre).

![[Pasted image 20260612142516.png]]

## **6. L'Évolution : ResNet et les Connexions Résiduelles (Skip Connections)**

Le problème des réseaux très profonds (ex: 50 couches de convolution) est la disparition du gradient lors de la rétropropagation : l'information se perd en traversant trop de couches successives. L'architecture **ResNet (Residual Network)** a révolutionné la vision en introduisant les _Skip Connections_ (connexions coupe-circuit).

- **Le Mécanisme :** Au lieu de faire passer l'information uniquement de la couche L à la couche L+1, l'entrée d'un bloc est additionnée directement à sa sortie : F(x)+x.

- **L'Avantage :** Le signal d'erreur de la _Backpropagation_ a désormais une "autoroute" pour remonter intact vers les premières couches du réseau. Cela a permis d'entraîner des réseaux de plus de 100 couches sans perte de performance.