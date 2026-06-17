## 1. L'Objectif : Contextualiser le Sens

À la fin de la Fiche [[20_Positional_Encoding]], nous avons des vecteurs qui contiennent le sens du mot (Embedding) et sa position (Positional Encoding). Mais ces vecteurs sont encore isolés. Dans la phrase _"L'avocat est mûr"_, le mot "avocat" a toujours son vecteur générique.

Le mécanisme de **Self-Attention** va permettre à chaque mot de "regarder" tous les autres mots de la phrase, de déterminer lesquels sont importants pour lui, et de **modifier ses propres coordonnées** pour affiner son sens.

## 2. L'Analogie de la Base de Données (Q, K, V)

Pour que les mots communiquent, le Transformer utilise trois concepts directement inspirés des moteurs de recherche : **Query** (Requête), **Key** (Clé) et **Value** (Valeur).

Pour chaque mot, le réseau crée trois nouveaux vecteurs (en multipliant le vecteur initial du mot par trois matrices de poids apprenables : $W_Q, W_K, W_V$) :

- **Query (Q) : "Ce que je cherche."** * Exemple pour l'adjectif _mûr_ : "Je suis un adjectif qui décrit un état, je cherche un nom commun comestible."

- **Key (K) : "Ce que je suis."** * Exemple pour le mot _avocat_ : "Je suis un nom commun, qui peut être un fruit ou un métier."

- **Value (V) : "Mon sens profond."** * C'est l'essence du mot, ce qu'il va transmettre aux autres s'ils décident de lui prêter attention.

## 3. Le Match Géométrique (Le Produit Scalaire)

Comment le modèle décide-t-il que la Query de _mûr_ correspond à la Key de _avocat_ ? Il utilise la géométrie, et plus précisément le **produit scalaire** (Dot Product).

Le produit scalaire entre deux vecteurs mesure leur alignement :

- Si la Query de _mûr_ et la Key de _avocat_ pointent dans la **même direction**, le résultat est un nombre très grand (Match fort).
    
- S'ils sont orthogonaux (perpendiculaires), le résultat est nul (Aucun lien).
    
- S'ils pointent dans des directions opposées, le résultat est négatif.
    

Le modèle calcule le produit scalaire de la Query d'un mot avec les Keys de **tous les autres mots** de la phrase. Il obtient ainsi un score d'attention brut pour chaque relation possible.

## 4. La Mise à Jour du Vecteur (Le Déplacement)

Une fois les scores calculés (et transformés en pourcentages via une fonction Softmax, que l'on verra à la fiche [[22_Softmax_et_Multi_Head]]), comment le mot se met-il à jour ?

Si le mot _mûr_ accorde 90% de son attention au mot _avocat_ et 10% au mot _est_ :

1. On prend 90% du vecteur **Value (V)** de _avocat_.

2. On prend 10% du vecteur **Value (V)** de _est_.

3. On additionne ces fragments de vecteurs au vecteur initial du mot _mûr_.

**L'intuition géométrique :** Le vecteur initial du mot _mûr_ va être physiquement **tiré** dans l'espace à travers les milliers de dimensions, en direction de l'espace sémantique des "fruits", parce qu'il a absorbé la "Valeur" du mot _avocat_.

Le vecteur mis à jour n'est plus le concept générique de "mûr", c'est devenu le concept ultra-spécifique de "la maturité d'un avocat". Le mot a été contextualisé.

![[Pasted image 20260617153035.png]]