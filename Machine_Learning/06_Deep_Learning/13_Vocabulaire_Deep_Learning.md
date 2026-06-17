## 1. Le Vocabulaire de la Boucle d'Entraînement

L'entraînement d'un réseau de neurones est itératif. Il faut maîtriser ces trois termes pour comprendre comment les données sont injectées dans le modèle :

- **Batch Size (Taille du lot) :** Le nombre d'échantillons (ex : $32$ images) traités simultanément par le réseau avant d'effectuer une mise à jour des poids. Un grand Batch Size accélère l'entraînement mais consomme beaucoup de mémoire vidéo.

- **Itération (ou Step) :** Une mise à jour effective des poids (un _Forward pass_ + un _Backward pass_). Le traitement mathématique d'un Batch complet équivaut à $1$ Itération.

- **Époque (Epoch) :** Un passage complet de l'intégralité du jeu de données d'entraînement à travers le réseau.
    
    - _Exemple :_ Si le dataset contient $1\ 000$ images et que le Batch Size est de $100$, une Époque nécessitera exactement $10$ Itérations.

## 2. Techniques Avancées et Régularisation

- **Early Stopping (Arrêt Prématuré) :** Méthode consistant à stopper l'entraînement avant la fin des époques prévues pour éviter l'Overfitting. On surveille la courbe d'erreur sur le jeu de validation : si elle arrête de descendre et commence à remonter pendant un certain nombre d'époques (paramètre appelé _Patience_), l'entraînement s'arrête et on restaure les poids de la meilleure époque.

- **Data Augmentation (Augmentation de données) :** Création artificielle de nouvelles données d'entraînement par déformation des données existantes (rotation, zoom, recadrage, ajout de bruit). Cela empêche le réseau d'apprendre les images par cœur et l'oblige à se concentrer sur les caractéristiques invariantes (un chat reste un chat même s'il est de travers).

- **Transfer Learning (Apprentissage par Transfert) :** Technique consistant à prendre un modèle de Deep Learning géant, déjà pré-entraîné par des chercheurs sur des millions de données (ex : ResNet), et à l'adapter à son propre problème. On "gèle" d'abord les premières couches du réseau (qui ont déjà appris à reconnaître des formes et des textures), puis on ne réentraîne que la toute dernière couche sur notre petit jeu de données spécifique (c'est le _Fine-Tuning_).

## 3. Les Normalisations Internes (Activation Normalization)

De la même manière que l'on applique un traitement de mise à l'échelle sur les données brutes avant de les injecter dans le réseau (voir Fiche [[05_Mise_A_L_Echelle]]), les valeurs d'activation internes des couches cachées du réseau doivent être stabilisées et recentrées tout au long de l'entraînement.

Dans un réseau profond (Deep Learning), les valeurs se déforment et se décalent au fil des couches. On utilise des normes internes pour recentrer les tenseurs $(N, C, L)$ en cours de route (où $N = \text{Batch}$, $C = \text{Canaux/Features}$, $L = \text{Longueur de séquence ou dimension spatiale}$).

- **Batch Normalization (BN) :** Normalise à travers le Batch ($N$).
    
    - _Cas d'usage :_ Réseaux convolutifs (CNN) classiques.
    
    - _Problème :_ Très instable si la taille du batch est petite. Catastrophique pour le NLP (texte) car les phrases ont des longueurs variables, ce qui casse le calcul de la moyenne sur le batch.

- **Layer Normalization (LN) :** Normalise à travers les caractéristiques ($C$) pour chaque élément individuellement.
    
    - _Cas d'usage :_ Transformers (NLP) et RNN.
    
    - _Avantage :_ Totalement indépendante de la taille du batch et de la longueur de la phrase.

- **Instance Normalization (IN) :** Normalise uniquement la dimension spatiale/temporelle ($L$) pour chaque canal.
    
    - _Cas d'usage :_ Transfert de style (GANs) pour supprimer le contraste/style global tout en gardant la structure.

- **Group Normalization (GN) :** Divise les canaux en groupes.
    
    - _Cas d'usage :_ Modèles de vision très lourds (segmentation médicale, détection HD) où le GPU ne permet de traiter qu'une ou deux images à la fois (petit batch).

### Synthèse comparative des Normalisations Internes

|Type|Dimension calculée|Domaine principal|Sensible au Batch ?|
|---|---|---|---|
|**Batch Norm**|$(N, L)$|Vision (CNN)|**Oui** (Instable si batch < 4)|
|**Layer Norm**|$(C)$|NLP (Transformers)|**Non**|
|**Instance Norm**|$(L)$|GANs (Style Transfer)|**Non**|
|**Group Norm**|Groupes de $(C)$|Vision (Petits Batchs)|**Non**|