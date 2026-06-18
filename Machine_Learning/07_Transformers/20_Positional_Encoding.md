## 1. Le Problème du "Sac de Mots" (Bag of Words)

Avant les Transformers, les modèles de langage utilisaient des **RNN** (Réseaux Récurrents). Les mots étaient lus **un par un**, de gauche à droite. Le réseau savait naturellement quel mot venait en premier. L'inconvénient ? C'était extrêmement lent à entraîner, impossible à paralléliser sur les cartes graphiques.

Le Transformer a été créé pour casser cette limite : il lit **tous les mots de la phrase en même temps** (en parallèle).

- **Le défaut de cette force :** S'il lit tout simultanément, il perd la notion de l'ordre. Pour lui, la phrase _"Le chien mord l'homme"_ est mathématiquement identique à _"L'homme mord le chien"_. C'est ce qu'on appelle un traitement en "sac de mots".

Pour qu'il comprenne la grammaire, il faut lui injecter artificiellement la notion d'ordre.

![Image_Bag_Of_Words](../00_Images/Pasted%20image%2020260617151534.png)

## 2. La Solution : L'Addition Géométrique

Puisque nous avons vu ([[19_Embeddings_Tokens]]) qu'un mot est un vecteur (une liste de nombres/coordonnées), la solution du Transformer est d'**ajouter un deuxième vecteur** à celui du mot : le vecteur de position.

$$\vec{Vecteur\_Final} = \vec{Embedding\_du\_Mot} + \vec{Vecteur\_de\_Position}$$

- Le vecteur d'Embedding contient le **sens** (ex: concept de chien).

- Le vecteur de Position contient la **place** (ex: 2ème mot de la phrase).

- En additionnant les deux, on déplace légèrement le vecteur dans l'espace. Le mot `chien` en position 2 n'aura pas tout à fait les mêmes coordonnées que le mot `chien` en position 8.

## 3. L'Intuition Géométrique (L'Horloge Multidimensionnelle)

Comment fabriquer ce fameux vecteur de position ? On ne peut pas juste ajouter `[1, 1, 1...]` pour le mot 1, et `[2, 2, 2...]` pour le mot 2. Les valeurs deviendraient gigantesques pour des textes longs, ce qui ferait exploser les calculs du réseau (instabilité du gradient).

Il faut une valeur qui reste bornée (entre -1 et 1) mais qui crée une "empreinte digitale" unique pour chaque position. Les créateurs du Transformer ont utilisé des **ondes sinusoïdales (Sinus et Cosinus)** entrelacées.

**L'analogie de l'horloge :** Imaginez un vecteur de position comme une série d'aiguilles tournant sur des cadrans à des vitesses différentes :

1. **La première dimension** (première coordonnée du vecteur) est comme l'aiguille des secondes : elle oscille extrêmement vite entre -1 et +1 en passant d'un mot au suivant.

2. **Les dimensions intermédiaires** sont comme l'aiguille des minutes : elles oscillent plus lentement.

3. **Les dernières dimensions** sont comme l'aiguille des heures : elles mettent des centaines de mots à faire un cycle complet.

Chaque position $p$ dans le texte correspond à un "arrêt sur image" de toutes ces aiguilles. La combinaison exacte des angles de toutes ces aiguilles crée un vecteur de position **absolument unique** pour chaque mot.

## 4. Pourquoi utiliser des ondes ? (La Magie des Positions Relatives)

Le choix des ondes n'est pas qu'une astuce pour garder des valeurs entre -1 et 1. C'est un choix géométrique brillant pour faciliter le calcul des **distances relatives**.

Dans la langue, la position absolue d'un mot ("c'est le 14ème mot") importe peu. Ce qui compte, c'est la distance relative ("l'adjectif est juste avant le nom").

Grâce aux propriétés trigonométriques des ondes, le réseau de neurones peut facilement calculer le décalage entre deux positions par de simples **rotations géométriques** (des transformations linéaires). Si le modèle veut trouver un mot situé à $K$ pas de distance du mot actuel, la mathématique des sinus/cosinus lui permet de "tourner" le vecteur de position de manière mathématiquement fluide, rendant l'apprentissage des règles grammaticales extrêmement efficace.

> **En résumé :** Le Positional Encoding permet au Transformer de traiter tous les mots en même temps à la vitesse de l'éclair (parallélisation), tout en conservant une connaissance spatiale géométrique de l'ordre de la phrase.