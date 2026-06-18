## 1. De la Biologie aux Mathématiques

Dans la fiche précédente, nous avons vu le comportement biologique complexe d'un neurone. Pour utiliser ce concept dans un réseau artificiel informatique, il faut le simplifier et le traduire en une équation calculable efficacement.

Le modèle **Leaky Integrate-and-Fire (LIF)** (Intégration et Fuite) est le standard de l'industrie. Il ignore la chimie complexe du cerveau pour se concentrer uniquement sur la dynamique électrique de la membrane, offrant le meilleur compromis entre réalisme et rapidité de calcul.

## 2. L'Analogie du Circuit Électrique (Circuit RC)

Le modèle LIF modélise la membrane du neurone comme un simple circuit électrique avec une résistance ($R$) et un condensateur ($C$) montés en parallèle.

* **Le Condensateur ($C$) :** Représente la capacité de la membrane à stocker la charge électrique entrante (la phase d'**Intégration**).
* **La Résistance ($R$) :** Représente les canaux ioniques qui laissent "fuir" la charge électrique au fil du temps (la phase de **Fuite**).
* **Le Courant ($I$) :** Représente les impulsions (*spikes*) reçues des autres neurones connectés en amont.

![Image_Circuit_RC](Fiches-IA-SNN/SNN/00_Images/Pasted%20image%2020260615140709.png)

## 3. L'Équation Différentielle Fondamentale

La dynamique de la tension membranaire $V(t)$ au fil du temps est régie par l'équation différentielle suivante :

$$\tau_m \frac{dV(t)}{dt} = -(V(t) - V_{rest}) + R \cdot I(t)$$

* **$V(t)$ :** Le potentiel de la membrane à l'instant $t$.
* **$V_{rest}$ :** Le potentiel de repos. Sans aucune stimulation extérieure ($I = 0$), la tension $V(t)$ tend inéluctablement vers cette valeur (la fuite).
* **$I(t)$ :** Le courant entrant à l'instant $t$.
* **$\tau_m$ (Tau) :** C'est la **constante de temps membranaire** ($\tau_m = R \cdot C$). C'est le paramètre le plus important du neurone LIF. Il définit la vitesse de la fuite. 
  * Un $\tau_m$ **élevé** signifie une fuite lente (le neurone a une bonne mémoire des stimuli passés).
  * Un $\tau_m$ **faible** signifie une fuite rapide (le neurone oublie vite, il lui faut des stimuli très rapprochés pour réagir).

![Image_LIF_structure](Fiches-IA-SNN/SNN/00_Images/Pasted%20image%2020260615171713.png)

## 4. Le Mécanisme de Décharge (Fire) et de Réinitialisation (Reset)

L'équation différentielle ci-dessus ne décrit que la pente douce de l'intégration. Elle ne modélise pas mathématiquement l'explosion du *Spike*. Pour générer le *Spike*, on ajoute une règle conditionnelle informatique stricte :

**La Règle du Seuil (Threshold) :**
Si la tension franchit le seuil, $V(t) \ge V_{th}$, alors deux choses se produisent instantanément :
1. **Fire :** Le neurone émet un Spike (valeur de sortie = $1$).
2. **Reset :** Le potentiel est instantanément écrasé et ramené à sa valeur de réinitialisation : $V(t) \to V_{reset}$ (généralement égal à $V_{rest}$).

![Image_LIF_Activation_System](Fiches-IA-SNN/SNN/00_Images/Pasted%20image%2020260615143550.png)

> **La difficulté mathématique :** C'est cette discontinuité brutale (le *Reset* vertical) qui pose un problème majeur. La dérivée d'une chute instantanée est indéfinie. Par conséquent, il est mathématiquement impossible d'appliquer l'algorithme de Rétropropagation (Backpropagation) classique sur un réseau LIF. Cela nous amènera au concept des *Surrogate Gradients* (Fiche [[06_Surrogate_Gradients_et_BPTT]]).

