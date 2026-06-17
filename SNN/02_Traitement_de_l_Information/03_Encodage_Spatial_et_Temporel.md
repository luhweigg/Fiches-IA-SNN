## 1. Le Problème de la Traduction (Le "Spike Encoding")

Les réseaux de neurones classiques (ANN) ingèrent des valeurs continues statiques (par exemple, un pixel gris d'une image valant `0.8`). 

Les réseaux neuromorphiques (SNN), en revanche, ne comprennent qu'une seule chose : des **impulsions binaires réparties dans le temps** (des *Spikes*, valant `1` ou `0` à un instant $t$).
Pour utiliser un SNN sur des jeux de données traditionnels (images, audio), il faut donc une étape de traduction préalable : l'encodage. Il existe quatre grandes stratégies.
## 2. Le Rate Coding (Encodage par Fréquence)

C'est la méthode la plus ancienne, directement inspirée des premières observations neurobiologiques (le cerveau code l'intensité d'une douleur par la fréquence des décharges nerveuses).

* **Le Principe :** L'intensité de la valeur d'entrée détermine la **fréquence** d'émission des spikes. On utilise souvent un processus de Poisson pour générer les impulsions de manière stochastique (aléatoire mais respectant une moyenne).
  * Un pixel très lumineux (ex: `250/255`) générera beaucoup de spikes (ex: 80 spikes sur une fenêtre de 100 ms).
  * Un pixel sombre (ex: `10/255`) générera très peu de spikes.
* **Avantage :** Extrêmement robuste au bruit. Si un spike est perdu, l'information globale (la moyenne) reste à peu près correcte.
* **Inconvénient :** Très lent et gourmand en énergie. Il faut "attendre" longtemps (une grande fenêtre temporelle) pour pouvoir calculer une fréquence fiable, et cela génère énormément de spikes, détruisant l'avantage énergétique des SNN.

![[Pasted image 20260615153146.png]]

## 3. Le Temporal Coding / Latency Coding (Time-To-First-Spike)

Pour pallier les défauts du Rate Coding, on utilise la dimension temporelle de manière beaucoup plus stricte. L'information n'est plus dans le nombre de spikes, mais dans le **timing exact** du spike.

* **Le Principe (TTFS - Time To First Spike) :** Chaque neurone d'entrée n'a le droit d'émettre qu'**un seul et unique spike**. L'intensité de l'entrée détermine quand ce spike est émis.
  * Un pixel très lumineux déclenchera un spike **très tôt** (latence faible).
  * Un pixel sombre déclenchera un spike **très tard** (latence forte).
* **Avantage :** Efficacité énergétique et vitesse maximales (Inférence ultra-rapide). Le réseau prend souvent sa décision en ne lisant que les tout premiers spikes qui arrivent.
* **Inconvénient :** Plus sensible au bruit temporel et nécessite une synchronisation d'horloge très précise en matériel.

![[Pasted image 20260615171259.png]]

![[Pasted image 20260615171414.png]]

## 4. La Delta Modulation (Encodage Événementiel)

C'est l'encodage naturel utilisé pour traiter les données issues des caméras neuromorphiques (DVS - Dynamic Vision Sensors) ou des capteurs audio spatiaux.

* **Le Principe :** On ne transmet pas l'intensité absolue, mais uniquement le **changement** d'intensité. Un spike n'est généré que si la variation de l'entrée dépasse un certain seuil.
  * Si la valeur augmente brusquement : Spike positif (ON événement).
  * Si la valeur baisse brusquement : Spike négatif (OFF événement).
  * Si la valeur est stable : Silence total (zéro spike).
* **Avantage :** Élimine toute la redondance spatiale et temporelle. C'est le Graal de l'efficacité neuromorphique (Sparse Data).

![[Pasted image 20260615171448.png]]

![[Pasted image 20260615171509.png]]

## 5. Le Population Coding (Encodage par Population)

Au lieu d'utiliser un seul neurone pour encoder une valeur, cette méthode distribue l'information sur un **groupe** de neurones. C'est biologiquement très réaliste (comme nos cônes rétiniens pour la couleur).

* **Le Principe :** Chaque neurone de la population possède un champ récepteur spécifique (souvent modélisé par une courbe de Gauss). Un neurone réagira fortement à une certaine plage de valeurs, et faiblement aux autres. Une valeur d'entrée unique va donc activer simultanément plusieurs neurones, mais à des intensités différentes. La valeur est codée par le *ratio* d'activation de la population entière.

![[Pasted image 20260615152722.png]]

* **Avantage :** Permet une précision extrême avec des neurones individuellement imprécis (bruités). Extrêmement robuste aux pannes (si un neurone meurt, la population globale conserve l'information).
* **Inconvénient :** Exige beaucoup plus de neurones (et donc de mémoire/poids synaptiques) pour encoder une seule variable.
## 6. Synthèse Comparative

| Méthode d'Encodage | Vecteur d'information | Avantage Principal | Inconvénient Principal | Cas d'usage idéal |
| :--- | :--- | :--- | :--- | :--- |
| **Rate Coding** | Le nombre de spikes sur une durée $T$ | Tolérance au bruit | Lent, surconsommation | Conversion basique ANN $\rightarrow$ SNN |
| **Temporal Coding** | L'instant d'un unique spike | Ultra-rapide, basse conso | Sensible au bruit | IA embarquée très réactive |
| **Delta Modulation** | Le changement d'état (ON/OFF) | Parcimonie (Sparsity) maximale | Nécessite des capteurs DVS | Traitement vidéo/audio temps réel |
| **Population Coding** | Le ratio d'activation d'un groupe | Précision et robustesse extrêmes | Gourmand en nombre de neurones | Encodage de variables continues (angles, positions) |
![[Pasted image 20260615171153.png]]