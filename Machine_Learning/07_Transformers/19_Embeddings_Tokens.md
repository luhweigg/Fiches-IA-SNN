## 1. La Tokenisation : Découper l'information

Un ordinateur ne possède aucune compréhension innée du texte ; il ne manipule que des nombres. La toute première étape d'un modèle de langage (Transformer) est donc de hacher le texte d'entrée en fragments appelés **Tokens**.

- **Qu'est-ce qu'un Token ?** Ce n'est pas systématiquement un mot entier. Un token peut être une syllabe, un préfixe, un mot très fréquent, ou même une simple lettre. Par exemple, le mot "indéniablement" pourrait être découpé en trois tokens : `["in", "déniable", "ment"]`.

- **Le Vocabulaire :** Le modèle possède un dictionnaire fini et figé (souvent entre 50 000 et 100 000 tokens). Lors de cette étape, chaque token est converti en son identifiant unique (un entier). Ex : `chat` = `1456`.

![Image_Tokenisation](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260617145143.png)

## 2. L'Espace des Embeddings

Attribuer un simple numéro d'identification (`1456`) à un mot est insuffisant : les entiers n'ont aucune relation de sens entre eux (le token `1457` n'a pas un sens mathématiquement proche de `1456`). C'est ici qu'intervient l'**Embedding** (le plongement lexical).

Il faut imaginer un espace mathématique gigantesque doté de milliers de dimensions (par exemple, 12 288 dimensions pour des modèles massifs).

- Chaque token du vocabulaire est projeté dans cet espace et devient **un vecteur** (une coordonnée ou une "flèche" à $N$ dimensions).

- **La Règle d'or : La direction encode le sens.** La position de ce vecteur n'est pas aléatoire. Le réseau a appris, lors de son entraînement, à regrouper les concepts. Deux mots ayant un sens similaire (ex : `Chien` et `Loup`) auront des vecteurs qui pointent dans des directions très proches.

![Image_Embedding](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260617145544.png)

## 3. L'Arithmétique des Mots

Puisque les mots sont devenus des entités géométriques (des flèches dans un espace), il devient possible d'effectuer des opérations mathématiques (additions, soustractions) sur le sens lui-même.

L'exemple canonique qui démontre la structuration de cet espace latent est l'équation suivante :

$$\vec{Roi} - \vec{Homme} + \vec{Femme} \approx \vec{Reine}$$

- **Interprétation géométrique :** Si vous prenez les coordonnées du vecteur `Roi`, que vous vous déplacez dans la direction opposée au concept de masculinité ($- \vec{Homme}$), puis que vous avancez dans la direction du concept de féminité ($+ \vec{Femme}$), vous atterrissez géométriquement dans une zone de l'espace où le vecteur le plus proche est le token `Reine`. L'espace a mathématiquement encodé la notion de genre et de royauté.

![Image_Graphique_Mots](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260617150100.png)

## 4. La Limite Fondamentale (Pourquoi l'Attention ?)

Le problème majeur de l'Embedding pur est qu'il est **totalement statique et aveugle à son contexte**. Dans le dictionnaire du modèle, le token `avocat` ne possède qu'un seul et unique vecteur, quelles que soient les circonstances.

- Phrase A : "J'ai mangé un _avocat_." (Concept : Fruit)

- Phrase B : "J'ai engagé un _avocat_." (Concept : Métier)

**La faille :** L'Embedding renverra exactement les mêmes coordonnées dans les deux phrases. Ce vecteur statique est une moyenne mathématique informe des deux concepts. Le mot est isolé, il ne sait pas quels sont les autres mots qui l'entourent.

Pour qu'un modèle comprenne réellement le texte, il faut que le vecteur `avocat` puisse s'adapter, se modifier dynamiquement en "regardant" les mots voisins (ex : le mot `mangé` va tirer le vecteur `avocat` vers la zone des fruits). C'est précisément le rôle du mécanisme de **Self-Attention**.