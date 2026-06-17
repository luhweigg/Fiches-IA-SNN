## 1. La 3ème Génération de Réseaux de Neurones

Jusqu'à présent (en Machine Learning et Deep Learning classique), nous utilisions des Réseaux de Neurones Artificiels (ANN). Bien qu'ils s'inspirent vaguement du cerveau, leur fonctionnement interne est fondamentalement différent de la biologie.

Les **SNN (Spiking Neural Networks - Réseaux de Neurones à Impulsions)** constituent ce qu'on appelle la 3ème génération d'IA. Leur but est de se rapprocher au maximum du fonctionnement réel du cerveau biologique pour en copier l'incroyable efficacité énergétique et la rapidité de traitement de l'information.

## 2. Le Changement de Paradigme : ANN vs SNN

La différence majeure réside dans **la nature de l'information** échangée et **la gestion du temps**.

| Caractéristique | ANN (Réseaux Classiques / Deep Learning) | SNN (Réseaux à Impulsions / Neuromorphiques) |
| :--- | :--- | :--- |
| **Nature de l'information** | Valeurs continues (Nombres à virgule/Floats, ex: `0.87`). | Binaire et discrète (Des "Spikes" / Impulsions : `0` ou `1`). |
| **Synchronisation** | **Synchrone :** Tous les neurones d'une couche calculent leur sortie en même temps à chaque cycle d'horloge. | **Asynchrone (Événementiel) :** Un neurone ne calcule et n'émet un signal *que* s'il reçoit suffisamment de stimuli. |
| **La notion de Temps** | Inexistante (sauf astuce mathématique via les séquences RNN/LSTM). Les calculs sont statiques. | **Native.** Le temps est une dimension physique. L'information est codée par *le moment précis* où le spike survient. |
| **Consommation d'Énergie**| Extrême (Matériels : GPU / TPU qui multiplient des matrices en permanence). | **Ultra-basse (Sparsity) :** Seuls les neurones actifs consomment de l'énergie. (Matériels : Puces neuromorphiques). |

![[Pasted image 20260612160703.png]]

## 3. L'Inspiration : Le Neurone Biologique

Pour comprendre un SNN, il faut comprendre un vrai neurone. Dans le cerveau, les neurones communiquent via des décharges électriques brèves appelées **Potentiels d'Action** (ou *Spikes*).

Le cycle de vie électrique d'un neurone suit ces étapes strictes :

1. **Potentiel de Repos :** Au repos, l'intérieur du neurone est chargé négativement par rapport à l'extérieur (environ **-70 mV**).

2. **Phase d'Intégration (Stimulation) :** Le neurone reçoit des neurotransmetteurs de ses voisins. Sa tension interne commence à monter lentement (il se dépolarise).

3. **Le Seuil (Threshold) :** Si la tension interne n'atteint pas un certain seuil critique (souvent autour de **-55 mV**), rien ne se passe, la tension retombe. L'information est perdue.

4. **Le Spike (Dépolarisation massive) :** Si le seuil est franchi, c'est la règle du "Tout ou Rien". Les canaux sodiques s'ouvrent brutalement, la tension bondit instantanément à **+40 mV**. C'est le fameux *Spike*. Cette impulsion électrique voyage le long de l'axone vers les autres neurones.

5. **Repolarisation et Période Réfractaire :** Après avoir "tiré" (*fired*), le neurone est épuisé. Sa tension chute drastiquement, descendant même en dessous de son niveau de repos (Hyperpolarisation à **-80 mV**). Pendant cette courte *période réfractaire*, le neurone est "sourd" et ne peut plus émettre de nouveau spike, quelle que soit la stimulation qu'il reçoit.

## 4. Pourquoi la Période Réfractaire est-elle cruciale ?

Dans les futurs modèles mathématiques (comme le LIF que nous verrons à la fiche [[02_Modeles_de_Neurones_LIF]]), implémenter cette période réfractaire est indispensable. Cela empêche le réseau de s'emballer (crise d'épilepsie mathématique) et limite la fréquence maximale de décharge, stabilisant ainsi l'apprentissage du réseau.