## 1. Comment décide-t-on d'envoyer un Spike ? (Le Processus de Poisson)

Dans l'encodage par fréquence (*Rate Coding*), on ne veut pas que le neurone tire de manière métronomique (ex: un spike toutes les 10 millisecondes exactement). Biologiquement, l'émission est stochastique (aléatoire), mais respecte une moyenne. Pour coder cela, on utilise les probabilités, et plus précisément un **Processus de Poisson**.

**L'algorithme étape par étape (à chaque instant $t$) :**

1. **Normalisation :** La valeur d'entrée (ex: un pixel valant `200` sur `255`) est convertie en une probabilité entre 0 et 1. Ici, $p = 200/255 \approx 0.78$.
2. **Tirage aléatoire :** À chaque pas de temps (chaque itération de la simulation), l'ordinateur génère un nombre aléatoire $r$ issu d'une distribution uniforme entre 0 et 1.
3. **La condition de Spike :** * Si $r < p$, alors on émet un **Spike (1)**.
   * Si $r \ge p$, alors **Rien (0)**.

**Conséquence :** Si $p = 0.9$ (pixel très clair), la probabilité que le tirage $r$ tombe en dessous de 0.9 est immense. Le neurone va générer un flux très dense de spikes. Si $p = 0.1$ (pixel sombre), il ne tirera presque jamais.

## 2. Le défi de la dimension temporelle dans le code

Dans un réseau de neurones classique (CNN), une image en niveaux de gris est un tenseur (une matrice) 3D : `[Batch, Hauteur, Largeur]`.
Dans un SNN, on ajoute la dimension du temps. Le tenseur devient 4D : `[Temps, Batch, Hauteur, Largeur]`.

*Exemple :* Pour une image de 28x28 pixels simulée sur 100 pas de temps, le SNN va générer un tenseur binaire (rempli de 0 et de 1) de taille `[100, 1, 28, 28]`. Le réseau va traiter cette image 100 fois de suite, en accumulant les spikes à travers l'équation LIF (Fiche [[02_Modeles_de_Neurones_LIF]]).

## 3. Comment gérer les images couleur (RGB) ?

Dans un modèle classique (Deep Learning), une image RGB ajoute une dimension "Canaux" (Channels = 3 pour Rouge, Vert, Bleu). Le tenseur classique est `[Batch, Canaux, Hauteur, Largeur]`.

Pour un SNN, on passe à **5 Dimensions** : `[Temps, Batch, Canaux, Hauteur, Largeur]`.

**Comment cela s'encode physiquement ?**
* On ne mélange pas les couleurs dans un seul neurone d'entrée. 

* Pour un seul pixel physique de ton image, le réseau SNN alloue **3 neurones d'entrée distincts** (un neurone sensible au Rouge, un au Vert, un au Bleu).

* Chacun de ces 3 neurones va appliquer son propre processus d'encodage (ex: TTFS ou Poisson) de manière totalement indépendante, en fonction de l'intensité de son canal spécifique.

* C'est la première couche cachée du réseau (souvent une couche de convolution) qui va recevoir ces 3 trains de spikes simultanés et apprendre à en extraire des combinaisons de couleurs complexes.