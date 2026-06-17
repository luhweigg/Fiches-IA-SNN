## 1. Le Mur Mathématique ("The Dead Neuron Problem")

Nous avons vu que pour entraîner un réseau profond classique, on utilise l'algorithme de Rétropropagation (*Backpropagation*), qui repose sur le calcul des dérivées (gradients) pour ajuster les poids.

**Le problème du SNN :** Le mécanisme de déclenchement d'un *Spike* est une fonction escalier (la fonction de Heaviside). 
* Si la tension $V < Seuil$, la sortie est $0$.
* Si la tension $V \ge Seuil$, la sortie saute instantanément à $1$.

**La conséquence catastrophique :** La dérivée d'une ligne plate horizontale (avant et après le seuil) est exactement égale à **zéro**. La dérivée d'un saut parfaitement vertical (au moment du seuil) est **infinie** (une impulsion de Dirac). 
Lorsque l'algorithme de rétropropagation tente de calculer l'erreur à travers cette fonction, le gradient est soit bloqué (zéro), soit il explose (infini). Le réseau n'apprend absolument rien. C'est le problème du "neurone mort".

## 2. La Solution : Les Gradients de Substitution (Surrogate Gradients)

Pour contourner ce mur théorique, les chercheurs ont inventé une astuce informatique brillante : on triche pendant la passe arrière (*Backward Pass*).

1. **Le Forward Pass (Inférence vraie) :** Lorsque la donnée avance dans le réseau pour faire une prédiction, on utilise la **vraie fonction escalier** stricte. Le réseau reste un pur SNN générant des impulsions binaires (0 ou 1). L'efficacité énergétique est préservée.

2. **Le Backward Pass (Apprentissage truqué) :**
   Lorsque l'erreur remonte le réseau pour mettre à jour les poids, on **remplace** la dérivée infinie de la fonction escalier par la dérivée d'une fonction continue "douce" (comme une fonction Sigmoïde ou un Arc Tangente). C'est le *Surrogate Gradient*.

> **Le résultat :** L'optimiseur (ex: Adam) reçoit un gradient propre, lisse et exploitable. Le réseau apprend de ses erreurs tout en restant formellement un réseau à impulsions.

![[Pasted image 20260615172504.png]]

## 3. L'Algorithme BPTT (Backpropagation Through Time)

Parce qu'un SNN intègre la notion de temps (à travers l'équation différentielle du LIF), la rétropropagation classique ne suffit pas. Il faut utiliser la **BPTT**, le même algorithme que pour les réseaux récurrents (RNN/LSTM - Fiche 16).

* **Le Déroulement (Unrolling) :** L'ordinateur "déroule" virtuellement le SNN sur tous les pas de temps de la simulation. Un réseau de 3 couches simulé sur 100 millisecondes est traité en mémoire comme un réseau gigantesque de 300 couches.
* **Le chemin du gradient :** L'erreur ne remonte pas seulement dans l'espace (de la couche 3 vers la couche 2), elle remonte aussi **dans le temps** (de la milliseconde 100 vers la milliseconde 99) via le potentiel de membrane qui a été mémorisé.

![[Pasted image 20260615172547.png]]

## 4. Avantages et Inconvénients du Surrogate Gradient

* **L'Avantage :** C'est la méthode la plus performante à ce jour. Elle permet aux SNN d'atteindre des niveaux de précision (Accuracy) comparables aux meilleurs CNN sur des tâches de vision ou d'audio complexes, tout en étant supervisée (contrairement à la STDP de la Fiche 04).
* **L'Inconvénient :** L'entraînement est extrêmement lourd et coûteux. Dérouler un réseau dans le temps via la BPTT exige une énorme quantité de mémoire vidéo (VRAM) sur les cartes graphiques. 

**Le compromis moderne :** On accepte de dépenser beaucoup d'énergie pendant quelques jours pour entraîner le modèle SNN sur de gros serveurs GPU avec les Surrogate Gradients. Une fois entraîné, le modèle est figé et déployé sur une puce neuromorphique (embarquée dans un drone ou un téléphone) où son exécution (inférence) consommera presque zéro énergie.