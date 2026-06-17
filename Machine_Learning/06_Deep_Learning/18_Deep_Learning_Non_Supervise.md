## 1. Les Autoencodeurs (AE)

Un autoencodeur est un réseau de neurones qui apprend à copier son entrée vers sa sortie, mais en passant par un "goulot d'étranglement" qui le force à compresser l'information.

**L'architecture en sablier :**
* **L'Encodeur :** Prend la donnée d'entrée $X$ (ex: une image) et la compresse en un vecteur de très petite dimension.

* **L'Espace Latent (Bottleneck) :** C'est le point le plus étroit du réseau. Il contient la représentation compressée de la donnée. Le réseau est forcé d'ignorer le bruit aléatoire pour ne conserver que les caractéristiques structurelles essentielles.

* **Le Décodeur :** Prend ce vecteur compressé et tente de reconstruire la donnée d'origine $\hat{X}$ avec le moins de perte possible.

**Cas d'usage :**
* **Débruitage :** Apprendre à reconstruire une image propre à partir d'une entrée corrompue ou bruitée.
* **Compression :** Stocker ou transmettre des données massives sous forme de vecteurs latents minuscules.
* **Détection d'anomalies :** Si le réseau ne parvient pas du tout à reconstruire une donnée, c'est qu'elle est anormale par rapport à la distribution normale qu'il a apprise.

## 2. Les Réseaux Antagonistes Génératifs (GAN)

Le GAN (Generative Adversarial Network) repose sur la compétition acharnée entre deux réseaux distincts. C'est un jeu à somme nulle (algorithme Minimax).

**La dualité créative :**
* **Le Générateur :** N'a jamais accès aux vraies données. Son but est de créer des données synthétiques (ex: de fausses images) à partir de bruit aléatoire, de manière assez réaliste pour tromper l'autre réseau.
* **Le Discriminateur :** Reçoit de manière aléatoire de vraies données (issues du dataset d'entraînement) et de fausses données (issues du Générateur). Son but est d'apprendre à différencier le vrai du faux (classification binaire).

**La boucle d'apprentissage :**
Plus le Discriminateur devient bon pour repérer les défauts, plus le Générateur est forcé d'ajuster ses poids pour créer des données hyper-réalistes afin de réussir à le tromper. L'entraînement s'arrête théoriquement à l'équilibre de Nash : lorsque le Générateur produit des données si parfaites que le Discriminateur a exactement 50% de chances de se tromper (il tire à pile ou face).

**Cas d'usage :**
* Création de visages humains inexistants mais photoréalistes.
* Transfert de style (appliquer le style de Van Gogh à une photographie).
* Génération de *Deepfakes*.

## 3. Comparaison Rapide : AE vs GAN

| Critère | Autoencodeurs (AE) | Réseaux Antagonistes (GAN) |
| :--- | :--- | :--- |
| **Objectif Principal** | Reconstruction et Compression d'informations | Génération de nouvelles données inédites |
| **Mécanisme d'Entraînement** | Direct (Minimisation de l'erreur de reconstruction) | Compétitif (Bataille entre deux réseaux) |
| **Qualité des images générées**| Souvent un peu floues (le réseau fait une moyenne) | Extrêmement nettes, texturées et réalistes |
| **Contrôle de l'Espace Latent** | Très structuré, continu et facile à manipuler | Parfois instable et difficile à contrôler |