## 1. Le Grand Assemblage (La circulation des vecteurs)

Un Transformer complet n'est rien d'autre qu'un empilement vertical de plusieurs blocs identiques (souvent appelés "couches"). Dans un modèle comme GPT-3, ce sont 96 blocs d'attention et de feed-forward qui sont empilés les uns sur les autres.

Voici le trajet exact d'une phrase à travers le réseau :

1. **La Base :** Les mots d'entrée sont tokenisés, convertis en vecteurs d'embeddings statiques, puis additionnés à leurs vecteurs de position (Fiches 01 & 02).

2. **Le Voyage :** Cet ensemble de vecteurs entre dans le premier bloc. Il subit l'Attention Multi-Tête (qui crée des connexions de sens), l'addition résiduelle, la LayerNorm, puis passe dans le réseau Feed-Forward (qui extrait les faits mémorisés), suivi d'une seconde addition et d'une normalisation (Fiches 03, 04 & 05).

3. **La Répétition :** Les vecteurs ainsi modifiés sortent du bloc 1 et entrent directement dans le bloc 2, puis le bloc 3, et ainsi de suite. À chaque couche, le sens s'affine et devient de plus en plus abstrait et contextualisé.

4. **La Sortie :** Les vecteurs finaux arrivent au sommet de la pile pour la prédiction.

## 2. Encodeur (BERT) vs Décodeur (GPT)

Bien que l'architecture originale de 2017 comprenait un Encodeur connecté à un Décodeur (pour la traduction d'une langue à une autre), la recherche moderne a séparé ces deux éléments en deux branches distinctes.

### A. L'Encodeur seul (Exemple : BERT)

- **La philosophie :** Comprendre le texte.

- **Le mécanisme d'attention :** Bidirectionnel. Chaque mot a le droit de regarder **tous** les autres mots de la phrase, qu'ils soient situés avant ou après lui.

- **Le cas d'usage :** Analyse de sentiment, classification de texte, recherche sémantique, extraction d'entités. Il est parfait pour analyser un texte déjà écrit et en extraire la substantifique moelle.

### B. Le Décodeur seul (Exemple : GPT)

- **La philosophie :** Générer du texte.

- **Le mécanisme d'attention :** Causal (ou masqué). Un mot n'a le droit de regarder que les mots situés **avant lui** dans la séquence. Il lui est strictement interdit de regarder vers le futur.

- **Le cas d'usage :** Génération de texte, agents conversationnels (Chatbots), rédaction assistée.

![Image_Encodeur_Decodeur](../00_Images/Pasted%20image%2020260617164502.png)

## 3. L'Astuce du Décodeur : Le Masquage (Causal Attention)

Comment force-t-on le Décodeur à ne regarder que le passé pendant son entraînement ? Si on lui laissait lire toute la phrase, il tricherait en copiant simplement les mots suivants pour prédire l'avenir, et n'apprendrait rien.

On utilise pour cela un **masquage mathématique** lors du calcul des scores d'attention, juste avant l'étape de la fonction Softmax (Fiche 04) :

1. On calcule la matrice de produit scalaire classique $Q \cdot K^T$.

2. On applique un masque triangulaire supérieur : tous les scores d'attention correspondant à des mots "futurs" sont écrasés et remplacés par la valeur $-\infty$ (moins l'infini).

3. Lors du passage au Softmax, l'exponentielle de $-\infty$ devient strictement égale à $0$.


$$\exp(-\infty) = 0$$

**Le résultat :** Les coefficients d'attention pour tous les mots futurs tombent à 0%. Les mots futurs deviennent mathématiquement invisibles. Le modèle est forcé d'apprendre à prédire la suite uniquement à partir des indices du passé.

## 4. L'Étape Finale : L'Unembedding et la Tête LM

Au sommet du dernier bloc du Décodeur, nous obtenons un vecteur contextualisé pour le dernier token de notre phrase. Comment transforme-t-on ce vecteur de $N$ dimensions (ex : 12 288 coordonnées) en un mot lisible par l'humain ?

On utilise la **Tête LM (Language Model Head)** :

1. **La Projection Linéaire (Unembedding) :** On multiplie le vecteur final par une matrice géante dont la taille correspond au vocabulaire du modèle (ex : une matrice de 12 288 lignes par 50 257 colonnes). Cette opération projette notre vecteur abstrait dans un espace géant où chaque dimension représente un mot du dictionnaire. On obtient un score brut (appelé _Logit_) pour chaque mot du dictionnaire.

2. **Le Softmax Final :** Ces scores bruts sont passés dans une fonction Softmax pour obtenir une distribution de probabilité propre (de 0 à 100%) sur l'intégralité des mots du vocabulaire.

3. **L'échantillonnage (Sampling & Température) :** Le modèle ne choisit pas toujours le mot qui a la probabilité la plus élevée (ce qui donnerait des textes très répétitifs et robotiques). Il effectue un tirage aléatoire pondéré par ces probabilités, ajusté par un paramètre de **Température** :
    
    - Une température basse (ex : $0.2$) aplatit les petites probabilités et favorise uniquement le grand favori (texte très logique mais prévisible).
    
    - Une température haute (ex : $0.9$) équilibre les chances, permettant des choix surprenants (créativité, mais risque de divagation).


Le mot choisi est alors réinjecté à la fin de la phrase d'origine, et tout le processus recommence pour générer le mot suivant, de manière auto-régressive.