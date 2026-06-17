## 1. La fonction Softmax

À l'issue de la Fiche [[21_Self_Attention_QKV]], le produit scalaire entre les _Queries_ (Q) et les _Keys_ (K) nous a donné des scores bruts. Le problème, c'est que ces scores peuvent être n'importe quoi : `-45`, `0`, `342`.

Pour que le réseau puisse doser la "quantité" de _Value_ (V) à absorber, il a besoin d'une distribution de probabilités propre (des pourcentages compris entre 0 et 1, dont la somme vaut exactement 1). C'est le rôle de la fonction **Softmax**.

**Le calcul mathématique final de l'attention :**

$$Attention(Q, K, V) = Softmax\left(\frac{Q \cdot K^T}{\sqrt{d_k}}\right) V$$

- **Pourquoi diviser par** $\sqrt{d_k}$ **(la racine de la dimension) ?** Dans un espace à très haute dimension, les produits scalaires peuvent produire des nombres gigantesques. Si on donne un nombre énorme au Softmax, il va écraser toutes les probabilités sur le mot gagnant (100% pour lui, 0% pour les autres). L'apprentissage serait totalement bloqué (gradient nul). La division "calme" les valeurs avant le Softmax pour garder une attention fluide et répartie.

![[Pasted image 20260617164204.png]]

## 2. La limite de la tête unique

Imaginons la phrase : _"Le **chat** a donné un coup de patte féroce au **chien**."_

Pour le mot `donné` (le verbe), l'attention doit se porter sur deux choses essentielles mais totalement différentes :

1. Le sujet (`chat`) pour comprendre _qui_ fait l'action.

2. L'objet (`chien`) pour comprendre _qui_ subit l'action.

Si le réseau ne possède qu'une seule matrice Q, K, V (une seule "tête d'attention"), le vecteur du mot `donné` va devoir faire une moyenne. Il va être tiré géométriquement pile entre le concept de chat et de chien. Cette moyenne détruit la spécificité grammaticale.

## 3. L'intuition géométrique du Multi-Head Attention

La solution de l'architecture Transformer est de **multiplier les points de vue**. Au lieu d'avoir un seul grand ensemble de matrices Q, K, V, on divise l'espace en plusieurs "têtes" parallèles (par exemple, 12 têtes ou 96 têtes pour les grands modèles).

- **Sous-espaces géométriques :** Chaque tête possède ses propres petites matrices $W_Q, W_K, W_V$. Elles projettent le vecteur du mot dans des sous-espaces mathématiques différents et indépendants.

- **Spécialisation :** En s'entraînant, les têtes se spécialisent naturellement, sans qu'on le leur demande.
    
    - La **Tête 1** va apprendre à se focaliser uniquement sur les relations "Sujet-Verbe".
    
    - La **Tête 2** va traquer les adjectifs qui modifient des noms.
    
    - La **Tête 3** va faire attention à la ponctuation ou aux rimes.

![[Pasted image 20260617153942.png]]

## 4. La Fusion (Concaténation et Projection Finale)

Une fois que toutes les têtes ont fait leur calcul d'attention dans leur coin, le mot `donné` a absorbé plein de petits morceaux de sens différents (un bout du vecteur _chat_, un bout du vecteur _chien_, un bout du vecteur _féroce_).

Comment recoller tout cela pour rendre au mot sa dimension d'origine ?

1. **Concaténation :** On met tous ces petits vecteurs de résultat bout à bout.

2. **Projection linéaire (**$W_O$**) :** On multiplie ce long vecteur résultant par une dernière matrice apprenable ($W_O$ pour _Output_).

Cette opération remixe toutes les "opinions" des différentes têtes pour produire le **vecteur mis à jour final**. Ce vecteur contient désormais une compréhension d'une richesse exceptionnelle de son contexte grammatical et sémantique exact.

![[Pasted image 20260617154011.png]]