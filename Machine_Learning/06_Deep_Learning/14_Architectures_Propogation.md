## 1. Définition et Extraction Automatique de Caractéristiques

Le Deep Learning (Apprentissage Profond) repose sur les Réseaux de Neurones Artificiels (ANN - Artificial Neural Networks). Sa spécificité majeure par rapport au Machine Learning traditionnel réside dans l'extraction automatique de caractéristiques (Representation Learning). 

Alors que le ML classique nécessite une phase manuelle d'ingénierie des variables (*Feature Engineering*, Fiche [[04_Nettoyage_Encodage]]), un réseau de neurones profond apprend des représentations de plus en plus abstraites et complexes des données brutes au fur et à mesure qu'elles traversent ses couches successives.

---

## 2. Le Neurone Artificiel (Perceptron)

Le neurone artificiel est l'unité mathématique de calcul élémentaire du réseau. Il effectue une combinaison linéaire de ses entrées, y ajoute un terme constant, puis applique une fonction non linéaire.

**Formalisation Mathématique**

Soit un neurone recevant un vecteur d'entrée $\mathbf{x} = [x_1, x_2, \dots, x_n]^T \in \mathbb{R}^n$. Le neurone est défini par un vecteur de **poids** $\mathbf{w} = [w_1, w_2, \dots, w_n]^T \in \mathbb{R}^n$ et un scalaire $b \in \mathbb{R}$ nommé **biais**.

La somme pondérée brute, notée $z$, est calculée via un produit scalaire :

$$
z = \sum_{i=1}^{n} (w_i \cdot x_i) + b = \mathbf{w}^T \mathbf{x} + b
$$

* **Les Poids ($\mathbf{w}$) :** Mesurent la force synaptique ou l'importance accordée à chaque entrée.
* **Le Biais ($b$) :** Permet de translater la fonction de coût et de contrôler le seuil d'activation du neurone indépendamment des valeurs d'entrée.

![Image_Perceptron](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612110639.png)

---

## 3. Les Fonctions d'Activation Non Linéaires

L'application de la seule somme pondérée $z$ constitue une opération linéaire. Une composition de fonctions linéaires restant strictement linéaire, un réseau composé uniquement de sommes pondérées équivaudrait mathématiquement à un simple modèle linéaire à une seule couche, quelle que soit sa profondeur :

$$
f(g(x)) = \mathbf{W}_2(\mathbf{W}_1\mathbf{x} + \mathbf{b}_1) + \mathbf{b}_2 = (\mathbf{W}_2\mathbf{W}_1)\mathbf{x} + (\mathbf{W}_2\mathbf{b}_1 + \mathbf{b}_2) = \mathbf{W}'\mathbf{x} + \mathbf{b}'
$$

Il est donc mathématiquement indispensable d'appliquer une fonction d'activation non linéaire $\sigma(z)$ à la sortie de la somme pondérée pour permettre au réseau d'approximer des fonctions non linéaires complexes (Théorème d'approximation universelle).

### A. Sigmoïde
Écrase les valeurs d'entrée dans l'intervalle ouvert $]0, 1[$.

$$
\sigma(z) = \frac{1}{1 + e^{-z}}
$$

* **Dérivée :** $\sigma'(z) = \sigma(z)(1 - \sigma(z))$
* **Limites :** Pour des valeurs de $|z|$ élevées, la fonction sature et sa dérivée tend vers 0. Cela bloque la rétropropagation des erreurs (phénomène de disparition du gradient ou *Vanishing Gradient*).

![Image_Sigmoide](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612110512.png)

### B. Tangente Hyperbolique (Tanh)
Écrase les valeurs d'entrée dans l'intervalle $]-1, 1[$.

$$
\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}
$$

* **Avantage :** Elle est centrée sur zéro, ce qui produit des sorties dont la moyenne est proche de 0, facilitant et stabilisant l'optimisation des couches suivantes. Elle souffre néanmoins du même problème de saturation que la sigmoïde.

![Image_Tanh](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612110848.png)

### C. ReLU (Rectified Linear Unit)
C'est le standard pour les couches cachées des réseaux profonds.

$$
\text{ReLU}(z) = \max(0, z)
$$

* **Dérivée :** $\text{ReLU}'(z) = 1$ si $z > 0$, et $0$ si $z < 0$.
* **Avantages :** Son calcul est extrêmement rapide (simple seuillage) et sa dérivée constante à 1 pour les valeurs positives supprime le problème de disparition du gradient sur ces activations.

![Image_ReLU](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612110903.png)

#### **Exemple de quelques autres fonctions d'activation :**

![Image_Fonctions_Activation](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612110638.png)

---

## 4. Architecture d'un Réseau Multicouche (MLP)

Un Perceptron Multicouche (MLP - Multi-Layer Perceptron) organise les neurones en plusieurs structures successives appelées couches (Layers).

1.  **Couche d'entrée (Input Layer) :** Reçoit le vecteur de caractéristiques initial $\mathbf{x}$. Aucun calcul n'est effectué ici.

2.  **Couches cachées (Hidden Layers) :** Couches intermédiaires réalisant les transformations non linéaires. Dans une couche dite **dense** (*Fully Connected*), chaque neurone est connecté à la totalité des neurones de la couche précédente.

3.  **Couche de sortie (Output Layer) :** Produit le vecteur de prédiction final $\hat{\mathbf{y}}$. 
    * *Régression :* Un seul neurone sans fonction d'activation (ou une fonction linéaire).
    * *Classification Multiclasse :* $C$ neurones associés à la fonction **Softmax** pour générer une distribution de probabilité sur les $C$ classes (la somme des sorties valant strictement 1) :
$$
        \text{Softmax}(z_i) = \frac{e^{z_i}}{\sum_{j=1}^{C} e^{z_j}}
        $$

![Image_Perceptron_Multicouche](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612111405.png)

---

## 5. Le Forward Pass (Propagation Avant)

Le *Forward Pass* correspond au cheminement des données depuis la couche d'entrée jusqu'à la couche de sortie pour générer une prédiction. D'un point de vue matriciel, les calculs d'une couche cachée $l$ se résument à :

$$
\mathbf{a}^{(l)} = \sigma\left(\mathbf{W}^{(l)} \mathbf{a}^{(l-1)} + \mathbf{b}^{(l)}\right)
$$

* $\mathbf{a}^{(l-1)}$ : Le vecteur d'activation (sorties) de la couche précédente (avec $\mathbf{a}^{(0)} = \mathbf{x}$).
* $\mathbf{W}^{(l)}$ : La matrice des poids de la couche $l$, de dimension $(\text{nombre de neurones de } l \times \text{nombre de neurones de } l-1)$.
* $\mathbf{b}^{(l)}$ : Le vecteur de biais de la couche $l$.
* $\mathbf{a}^{(l)}$ : Le vecteur d'activation résultant de la couche $l$.

![Image_Forward_Pass](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260612111623.png)