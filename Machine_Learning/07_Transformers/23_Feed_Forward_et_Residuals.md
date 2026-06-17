## 1. Le Danger de l'Attention : Perdre son identité

À l'issue de l'étape de _Self-Attention_ (Fiches [[21_Self_Attention_QKV]] et [[22_Softmax_et_Multi_Head]]), chaque mot a pompé le sens de ses voisins pour se contextualiser.

Le danger géométrique est grand : si le vecteur du mot "avocat" absorbe trop d'informations du mot "mangé" et du mot "délicieux", il risque de se transformer complètement en un vecteur générique de "nourriture" et de perdre son sens originel de "fruit vert à noyau".

Pour qu'un réseau profond fonctionne, un mot doit se contextualiser **sans oublier ce qu'il est à la base**.

## 2. Les Connexions Résiduelles

La solution à ce problème d'identité est d'une simplicité mathématique redoutable : la **Connexion Résiduelle** (le "Add" de la fameuse étape _Add & Norm_).

- **L'intuition :** Le bloc de Self-Attention ne calcule pas le nouveau vecteur absolu du mot. Il calcule **un vecteur de déplacement** (un $\Delta v$).

- **L'opération :** On prend le vecteur original du mot (avant l'attention) et on lui _additionne_ ce vecteur de déplacement.

$$\vec{V}_{contextualisé} = \vec{V}_{original} + \vec{V}_{deplacement\_attention}$$

Grâce à cette addition, le mot reste fondamentalement ancré dans sa zone sémantique d'origine, mais il fait un "pas de côté" géométrique dans la direction indiquée par son contexte.

**Bonus technique :** Cette connexion crée une "autoroute" ininterrompue pour le passage du gradient lors de l'entraînement (Backpropagation), évitant que le réseau ne meure d'effondrement géométrique (Vanishing Gradient).

## 3. La Normalisation (LayerNorm)

Après l'addition, les coordonnées du vecteur peuvent avoir gonflé. Si l'on empile des dizaines de couches de Transformers, ces additions successives feraient exploser les valeurs vers l'infini.

L'étape de **Layer Normalization** intervient pour recentrer mathématiquement le vecteur. Elle s'assure que la longueur globale de tous les vecteurs reste harmonieuse, pour que l'espace géométrique reste navigable à la couche suivante. 

Une description plus poussée est disponible dans les fiche [[05_Mise_A_L_Echelle]] et [[13_Vocabulaire_Deep_Learning]].


## 4. Le Réseau Feed-Forward (La Base de Connaissances)

C'est la deuxième moitié d'un bloc Transformer. Contrairement à l'Attention qui fait communiquer les mots entre eux, la couche **Feed-Forward** agit sur chaque vecteur de mot **de manière totalement individuelle et isolée**.

Pourquoi modifier un vecteur tout seul ? C'est ici que réside la véritable "mémoire" factuelle du réseau.

- **Le rôle de l'Attention :** Comprendre la grammaire et le contexte actuel de la phrase. (Ex: Comprendre que "Il" désigne "Louis XVI").

- **Le rôle du Feed-Forward :** Interroger les connaissances apprises pendant l'entraînement.

**L'intuition géométrique :** Une fois que le vecteur du mot "Il" a été tiré par l'Attention dans la zone sémantique exacte de "Le Roi de France exécuté en 1793", il entre dans le réseau Feed-Forward. Ce réseau agit comme un immense catalogue d'associations d'idées non-linéaires. Il va reconnaître cette coordonnée spatiale très précise et appliquer un dernier vecteur de déplacement pour pousser le mot vers le concept factuel associé : la "Guillotine".

## 5. La Structure de la Mémoire

Le Feed-Forward est un réseau de neurones multicouches classique (Perceptron / MLP), mais il a une forme bien particulière en deux étapes :

1. **L'Expansion :** Il projette le vecteur dans une dimension énorme (souvent 4 fois plus grande que l'espace d'Embedding initial). C'est là qu'il "déplie" le vecteur pour chercher des corrélations complexes dans sa mémoire.

2. **La Contraction :** Après avoir appliqué une fonction d'activation non-linéaire (ReLU ou GELU, qui active ou désactive certains "faits" de la mémoire), il reprojette le vecteur dans sa dimension normale pour l'envoyer à la couche Transformer suivante.