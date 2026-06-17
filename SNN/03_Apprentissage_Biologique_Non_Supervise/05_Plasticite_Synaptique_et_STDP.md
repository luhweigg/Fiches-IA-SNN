## 1. Le Problème de l'Apprentissage Classique

Dans le Deep Learning classique (ANN), l'apprentissage se fait via la **Rétropropagation du Gradient** (Backpropagation). C'est un algorithme :
* **Global :** Le calcul de l'erreur à la fin du réseau doit remonter à travers toutes les couches.
* **Supervisé :** Il a besoin d'étiquettes ($Y$) pour calculer l'erreur.
* **Mathématiquement incompatible avec les Spikes :** La dérivée d'un saut binaire (le Spike de 0 à 1) est indéfinie (ou infinie), ce qui bloque le calcul du gradient.

Le cerveau humain ne fonctionne pas du tout comme ça. Un neurone biologique n'a aucune idée de ce qui se passe à l'autre bout du cerveau. Il n'apprend que grâce à des informations **purement locales**.

## 2. Le Postulat de Hebb (1949)

Donald Hebb a formulé la règle d'or de la neuroscience computationnelle : 
> *"Des neurones qui s'activent ensemble se lient ensemble." (Neurons that fire together, wire together).*

**Le principe :** Si le Neurone A (pré-synaptique) participe de manière répétée et persistante à déclencher le Neurone B (post-synaptique), alors l'efficacité de la connexion (le **poids synaptique**, $W$) entre A et B augmente. C'est ce qu'on appelle la **Plasticité Synaptique**.

## 3. La Révolution : STDP (Spike-Timing-Dependent Plasticity)

La règle de Hebb était trop vague. Dans les années 90, les chercheurs ont découvert que ce n'est pas seulement le fait de s'activer "ensemble" qui compte, mais le **timing exact** à la milliseconde près entre les deux impulsions. 

La STDP est l'algorithme d'apprentissage non supervisé roi des SNN. Elle repose sur la causalité temporelle : $\Delta t = t_{post} - t_{pre}$

![[Pasted image 20260615172426.png]]

* **LTP (Long-Term Potentiation / Renforcement) :** Si le Neurone A tire juste *avant* le Neurone B ($\Delta t > 0$), cela signifie que A a probablement causé B. La connexion est jugée utile, le poids synaptique **augmente**.
* **LTD (Long-Term Depression / Affaiblissement) :** Si le Neurone A tire juste *après* le Neurone B ($\Delta t < 0$), cela signifie que A n'a joué aucun rôle dans l'activation de B. La connexion est jugée inutile, le poids synaptique **diminue**.

![[Pasted image 20260615171838.png]]

## 4. Les Mathématiques de la STDP

La modification du poids synaptique ($\Delta W$) dépend de l'écart temporel ($\Delta t$) selon une courbe asymétrique constituée de deux fonctions exponentielles :

$$\Delta W = \begin{cases} A_+ \exp(-\Delta t / \tau_+) & \text{si } \Delta t > 0 \text{ (LTP)} \\ -A_- \exp(\Delta t / \tau_-) & \text{si } \Delta t < 0 \text{ (LTD)} \end{cases}$$

* **$A_+$ et $A_-$ :** Les taux d'apprentissage (Learning rates) maximums pour le renforcement et l'affaiblissement.
* **$\tau_+$ et $\tau_-$ :** Les constantes de temps. Elles définissent la "fenêtre de tir". Si les spikes sont espacés de plus de 40 ms, $\Delta W \approx 0$ (aucune modification du poids, la causalité est trop faible).

## 5. Pourquoi utiliser la STDP en Machine Learning ?

* **Extraction de caractéristiques non supervisée :** Un réseau SNN doté de STDP et soumis à des millions d'images va naturellement et automatiquement ajuster ses poids pour s'activer sur les motifs visuels les plus fréquents (des bords, des angles), **sans aucune étiquette**.
* **Efficacité matérielle :** Comme la règle est purement locale (le neurone n'a besoin que de connaître l'heure de son propre spike et celui de son voisin direct), c'est extrêmement facile et économe en énergie à coder sur des puces neuromorphiques.

> **La limite actuelle :** Bien que magnifique biologiquement, la STDP seule ne permet pas d'atteindre les scores de précision colossaux des CNN modernes sur des tâches de classification complexes (comme ImageNet). C'est pourquoi la recherche tente de fusionner les SNN avec des méthodes supervisées (que nous verrons dans la Fiche 05).