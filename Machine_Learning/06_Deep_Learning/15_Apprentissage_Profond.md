## 1. L'Évaluation : La Fonction de Coût (Loss Function)

Pour qu'un réseau de neurones apprenne, il doit d'abord être capable de mesurer ses propres erreurs. À la fin du *Forward Pass* (Fiche [[14_Architectures_Propogation]]), la prédiction $\hat{y}$ est comparée à la vraie valeur $y$ via une **Fonction de Coût** (souvent notée $L$ pour *Loss* ou $J$).

Le choix de cette fonction dépend strictement de la nature du problème :

* **Régression (Prédire une valeur continue) :** On utilise généralement la **MSE** (Erreur Quadratique Moyenne).
  $$L(y, \hat{y}) = \frac{1}{N} \sum_{i=1}^{N} (y_i - \hat{y}_i)^2$$

* **Classification Multiclasse (Prédire une catégorie) :** On utilise l'**Entropie Croisée Catégorielle (Categorical Cross-Entropy)**. Elle mesure la distance entre la distribution de probabilités prédite (la sortie du *Softmax*) et la vraie distribution (un vecteur binaire *One-Hot*).
  $$L(y, \hat{y}) = - \sum_{c=1}^{C} y_c \log(\hat{y}_c)$$
  *(où $y_c$ vaut 1 pour la vraie classe et 0 pour les autres, forçant le modèle à maximiser la probabilité $\hat{y}_c$ de la bonne classe).*

## 2. La Rétropropagation (Backpropagation)

L'objectif de l'apprentissage est de trouver l'ensemble des poids $W$ et biais $B$ qui minimise la fonction de coût $L$. Pour cela, on utilise la Descente de Gradient (vue en Fiche [[06_Regression]]).

**Le problème :** Comment calculer le gradient (l'impact sur l'erreur finale) d'un poids $w_1$ situé dans la toute première couche d'un réseau qui en compte 50 ? L'erreur finale est le résultat d'une immense cascade de fonctions imbriquées.

**La solution :** L'algorithme de Rétropropagation du Gradient. Il repose sur le théorème de dérivation des fonctions composées (**Chain Rule**). L'algorithme calcule le gradient de l'erreur en partant de la dernière couche (la sortie), puis "recule" couche par couche vers l'entrée.

Pour un poids $w_{ij}$ connectant un neurone $i$ à un neurone $j$, on calcule la dérivée partielle de l'erreur par rapport à ce poids en multipliant les gradients locaux successifs :

$$
\frac{\partial L}{\partial w_{ij}} = \frac{\partial L}{\partial a_j} \cdot \frac{\partial a_j}{\partial z_j} \cdot \frac{\partial z_j}{\partial w_{ij}}
$$

* $\frac{\partial L}{\partial a_j}$ : Comment l'activation du neurone $j$ impacte l'erreur finale.
* $\frac{\partial a_j}{\partial z_j}$ : La dérivée de la fonction d'activation (ex: dérivée de ReLU).
* $\frac{\partial z_j}{\partial w_{ij}}$ : L'impact du poids sur la somme pondérée (qui est simplement l'activation de l'entrée $a_i$).

Une fois ce gradient calculé pour **tous** les poids du réseau, on peut les mettre à jour.

![Image_Backpropagation](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612113345.png)

## 3. Les Optimiseurs et la Mise à Jour des Poids

L'optimiseur est l'algorithme qui utilise les gradients calculés par la Rétropropagation pour ajuster les poids. L'équation de base est :

$$
w_{nouveau} = w_{ancien} - \alpha \frac{\partial L}{\partial w}
$$

Le paramètre $\alpha$ (Alpha) est le **Taux d'Apprentissage (Learning Rate)**. C'est l'hyperparamètre le plus critique en Deep Learning.

![Image_Learning_Rate](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612140700.png)

### A. Descente de Gradient Classique (Batch GD)
Calcule l'erreur sur l'**intégralité** du jeu de données (des millions d'images) avant de faire une seule mise à jour des poids.
* **Avantage :** Trajectoire très lisse et directe vers le minimum.
* **Inconvénient :** Extrêmement lent et sature la mémoire de la carte graphique (VRAM). Peut bloquer dans des minimums locaux.

### B. SGD (Stochastic Gradient Descent)
Met à jour les poids après chaque **unique** donnée (ex: image par image).
* **Avantage :** Calcul ultra-rapide et l'aspect chaotique aide à "sauter" hors des minimums locaux.
* **Inconvénient :** La trajectoire est tellement bruyante (erratique) qu'elle peine à converger proprement à la fin. Ne profite pas de la puissance de calcul parallèle des GPU.

### C. Mini-Batch Gradient Descent
C'est le compromis standard. Divise le jeu de données en petits sous-ensembles de taille fixe (les *mini-batches*, souvent 32, 64 ou 128 données). L'algorithme calcule le gradient moyen sur ce lot et met à jour les poids.
* **Avantage :** Trajectoire plus stable que le SGD pur, tout en permettant une parallélisation massive des calculs matriciels sur carte graphique (GPU). Évite de saturer la mémoire.
* **Inconvénient :** Introduit un nouvel hyperparamètre à paramétrer manuellement (la taille du *batch*).

![Image_Comparatif_Descente](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612140436.png)

### D. SGD avec Momentum
Ajoute une notion d'inertie physique à la descente. L'algorithme accumule une fraction des gradients passés pour déterminer la direction actuelle.
* **Avantage :** Amortit les oscillations du SGD/Mini-Batch pur et accélère considérablement la descente dans les zones plates ou les ravins.

### E. Adagrad (Adaptive Gradient Algorithm)
Adapte le taux d'apprentissage $\alpha$ de manière **individuelle pour chaque paramètre**.
* **Principe :** Il divise $\alpha$ par la racine carrée de la somme des carrés de tous les gradients passés de ce paramètre. Les paramètres qui reçoivent de grands gradients voient leur taux d'apprentissage baisser rapidement, tandis que ceux avec de petits gradients ont un taux qui baisse plus lentement.
* **Avantage :** Excellent pour les données creuses (*sparse data*).
* **Inconvénient :** L'accumulation continue au dénominateur fait que le taux d'apprentissage tend inéluctablement vers 0. Le réseau s'arrête d'apprendre prématurément.

### F. RMSprop (Root Mean Square Propagation)
Conçu spécifiquement pour corriger le défaut d'Adagrad.
* **Principe :** Au lieu d'accumuler *tous* les gradients passés à l'infini, il utilise une moyenne mobile exponentielle. Il "oublie" l'historique trop ancien pour ne garder que la tendance récente.
* **Avantage :** Maintient un taux d'apprentissage par paramètre sans qu'il ne s'effondre vers zéro. Très efficace et historiquement populaire pour les réseaux récurrents (RNN).

### G. Adam (Adaptive Moment Estimation)
Le standard industriel absolu aujourd'hui.
* **Principe :** C'est la fusion entre le **Momentum** (qui garde une moyenne des gradients passés) et **RMSprop** (qui garde une moyenne des carrés des gradients passés).
* **Résultat :** Une convergence drastiquement plus rapide et robuste. Il nécessite beaucoup moins d'ajustements manuels du taux d'apprentissage initial. Si un paramètre oscille trop, Adam réduit automatiquement son taux ; s'il progresse constamment dans la même direction, Adam l'accélère.

### H. AdamW (Adam with Weight Decay) 
C'est la version corrigée et le standard absolu actuel (notamment pour les Transformers/LLMs).

- **Le problème d'Adam classique :** Lorsqu'on applique une régularisation L2 (Fiche [[12_Hyperparametres_Regularisation]]) avec l'optimiseur Adam classique, la pénalité est mélangée au calcul de la moyenne mobile des gradients. Cela fausse la régularisation et rend le modèle sous-optimal.

- **La solution AdamW :** Il découple mathématiquement la pénalité (Weight Decay) de la mise à jour du gradient. Le modèle bénéficie ainsi de la vitesse d'Adam ET de la robustesse de la régularisation L2, permettant une bien meilleure généralisation.

![Image_Comparatif_Optimisateurs](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612140701.png)

## 4. Les Schedulers (Planificateurs)

Avoir un Taux d'Apprentissage (α ou _Learning Rate_) fixe tout au long de l'entraînement est une mauvaise stratégie. S'il est trop grand, le modèle converge vite mais oscille brutalement autour du minimum final sans jamais l'atteindre. S'il est trop petit, le modèle met un temps infini à apprendre.

Le **Scheduler** est un algorithme qui modifie dynamiquement la valeur de α au fil des époques (itérations) pour optimiser la trajectoire de l'optimiseur.

**Les stratégies principales :**

- **Step Decay (Paliers) :** Divise le taux d'apprentissage par un facteur (ex: par 10) tous les N époques. Le modèle avance à grands pas au début, puis fait des pas minuscules à la fin pour se "poser" précisément au fond du minimum local.

- **Cosine Annealing :** Fait décroître le taux d'apprentissage en suivant la courbe d'une fonction cosinus (baisse lente au début, chute rapide au milieu, et atterrissage très doux à la fin).

- **Warmup (Échauffement) :** Très utilisé avec AdamW. Au tout début de l'entraînement (quand les poids sont aléatoires et les gradients chaotiques), on commence avec un taux d'apprentissage proche de zéro, qu'on augmente linéairement pendant quelques époques. Cela évite au modèle de diverger et de "crasher" dans les premiers instants de l'entraînement.