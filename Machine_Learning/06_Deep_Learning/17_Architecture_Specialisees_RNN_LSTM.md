## 1. Le Défi des Données Séquentielles et Temporelles

Jusqu'à présent, les réseaux (MLP, CNN) traitaient des données statiques de taille fixe. Mais de nombreuses données dans le monde réel sont des séquences : le texte (une suite de mots), l'audio (une suite d'amplitudes), ou la bourse (une série temporelle).

Le traitement séquentiel pose deux défis mathématiques inédits :

1. **La longueur variable :** Une phrase peut faire 3 mots ou 300 mots. Un réseau classique exige une taille d'entrée strictement fixe.

2. **L'importance de l'ordre (Le Temps) :** La séquence "A $\rightarrow$ B" n'a pas le même sens que "B $\rightarrow$ A". Le réseau doit posséder une notion de chronologie et donc, une **mémoire**.

## 2. L'Encodage des Éléments (L'Embedding)

Avant de traiter une séquence temporelle comme du texte, l'ordinateur doit numériser chaque élément. Le *One-Hot Encoding* (Fiche [[04_Nettoyage_Encodage]]) crée des vecteurs immenses, creux, et sans aucune notion de sens.

On utilise donc une couche d'**Embedding** (Plongement lexical). 

* **Le Principe :** Chaque mot du vocabulaire est projeté dans un espace vectoriel continu et dense (ex: de dimension 300).

* **La Propriété :** L'algorithme d'Embedding (comme *Word2Vec*) apprend à placer les mots sémantiquement proches dans des zones géométriques proches. Le réseau ne lit plus des "mots", mais ingère une trajectoire de points dans un espace mathématique.

![Image_Embedding](../00_Images/Pasted%20image%2020260612143530.png)

## 3. Le Réseau de Neurones Récurrent (RNN)

Pour lire une séquence, le RNN (Recurrent Neural Network) introduit une boucle interne. Il ne traite plus toute la donnée d'un coup, mais "pas à pas" (à chaque instant $t$).

Pour ce faire, il possède un **État Caché (Hidden State)**, noté $h$, qui agit comme la mémoire de la phrase en cours de lecture.

**Fonctionnement mathématique à l'instant $t$ :**
Le réseau calcule son nouvel état caché $h_t$ en combinant l'entrée actuelle $x_t$ (le mot en cours) ET son état caché précédent $h_{t-1}$ (le résumé de tout ce qu'il a lu avant).

$$h_t = \tanh(W_x x_t + W_h h_{t-1} + b)$$

* $x_t$ : L'entrée à l'instant $t$.
* $h_{t-1}$ : La mémoire de l'instant précédent.
* $W_x$ et $W_h$ : Les matrices de poids (qui sont **partagées** et restent identiques à chaque pas de temps).
* La fonction $\tanh$ maintient les valeurs de la mémoire entre -1 et 1 pour éviter qu'elles n'explosent au fil du temps.

![Image_RNN](../00_Images/Pasted%20image%2020260612143855.png)

## 4. La Limite du RNN : L'Amnésie (Vanishing Gradient)

Pour corriger ses erreurs, le RNN utilise la **Rétropropagation à travers le temps (BPTT - Backpropagation Through Time)**. L'algorithme déroule la boucle temporelle et calcule les gradients du dernier mot jusqu'au premier.

**Le problème mathématique :** Lors de la dérivation en chaîne sur de longues séquences (ex: 50 mots), on multiplie la matrice des poids $W_h$ par elle-même 50 fois. Si les valeurs propres de cette matrice sont inférieures à 1, le gradient final tend exponentiellement vers 0. 
* **Conséquence :** Le réseau est incapable d'ajuster ses poids pour relier une information située au début de la phrase à une prédiction située à la fin. Le RNN basique a une mémoire à très court terme (amnésie).

## 5. La Solution : Le LSTM (Long Short-Term Memory)

Pour résoudre cette amnésie, le LSTM remplace le neurone RNN basique par une "Cellule" complexe. Sa grande innovation est de séparer l'État Caché ($h_t$, la mémoire à court terme) de l'**État de la Cellule ($C_t$, la mémoire à long terme)**.

Le $C_t$ agit comme une "autoroute de l'information" qui traverse toute la séquence sans être écrasée par des multiplications matricielles complexes, permettant au gradient de circuler librement.

L'ajout ou la suppression d'informations sur cette autoroute est strictement régulé par trois "Portes" (*Gates*), composées de fonctions Sigmoïdes (qui sortent une valeur entre 0 et 1, agissant comme des valves) :

1. **Forget Gate (Porte d'oubli) :** Regarde $h_{t-1}$ et $x_t$, et décide (entre 0 et 1) quelle proportion de l'ancienne mémoire à long terme $C_{t-1}$ doit être définitivement effacée (ex: oublier le genre d'un personnage quand on commence un nouveau chapitre).
2. **Input Gate (Porte d'entrée) :** Décide quelle nouvelle information issue du mot actuel $x_t$ mérite d'être ajoutée à la mémoire à long terme $C_t$.
3. **Output Gate (Porte de sortie) :** Filtre la mémoire à long terme $C_t$ pour générer le nouvel état caché $h_t$ (la mémoire à court terme) qui sera passé à l'étape suivante.

![Image_LSTM](../00_Images/Pasted%20image%2020260612144140.png)