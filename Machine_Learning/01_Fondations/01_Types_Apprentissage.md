## 1. Différenciation : IA vs Machine Learning vs Deep Learning

| Concept | Définition |
| :--- | :--- |
| **Intelligence Artificielle (IA)** | Tout système capable de reproduire des fonctions cognitives humaines (raisonnement, résolution de problèmes). Inclut les systèmes basés sur des règles explicites (systèmes experts). |
| **Machine Learning (ML)** | Sous-domaine de l'IA. Les algorithmes apprennent des modèles à partir de données sans être explicitement programmés pour chaque règle. |
| **Deep Learning (DL)** | Sous-domaine du ML basé sur des réseaux de neurones artificiels comportant de multiples couches (*deep layers*) pour extraire des caractéristiques de haut niveau. |

---

## 2. Apprentissage Supervisé (Supervised Learning)

**Le principe :** L'algorithme apprend à partir d'un ensemble de données étiquetées.

**L'objectif :** Prédire la bonne étiquette (la cible) pour de nouvelles données que la machine n'a jamais vues.

**Les deux grandes familles :**
* **La classification :** La prédiction est une catégorie
	* *Exemple :* Prédire si un email est "Spam" ou "Non Spam".

- **La régression :** La prédiction est une valeur continue
	- *Exemple :* Prédire le prix d'une maison en fonction de sa surface.

![Image_Apprentissage_Supervise](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611104408.png)

---

## 3. Apprentissage Non Supervisé (Unsupervised Learning)

**Le principe :** On fournit à la machine des données brutes, sans aucune étiquette. On lui dit pas ce qu'elle doit chercher.

**L'objectif :** Découvrir par elle-même la structure cachée, les motifs ou les relations naturelles dans les données.

**Les deux grandes familles :**
* **Le Clustering (Regroupement) :** L'algorithme regroupe les données similaires en clusters.
    * *Exemple :* Segmenter automatiquement les clients d'un supermarché en groupes (les acheteurs du dimanche, les acheteurs bio, etc.) selon leurs tickets de caisse, sans avoir défini ces groupes à l'avance.

* **La Réduction de Dimensionnalité :** L'algorithme simplifie les données complexes en éliminant le superflu tout en gardant l'information importante.
    * *Exemple :* Résumer un tableau de 500 colonnes en seulement 5 colonnes essentielles pour le rendre analysable ou visualisable.

![Image_Apprentissage_Non_Supervise](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611104409.png)

---

### 4. Apprentissage Semi-Supervisé (Semi-Supervised Learning)

- **Le principe :** L'algorithme s'entraîne sur un jeu de données hybride contenant une très petite quantité de données étiquetées et une immense quantité de données brutes (non étiquetées).

- **L'objectif :** Obtenir des performances de prédiction élevées lorsque l'étiquetage manuel par des experts humains est trop coûteux, lent ou impossible à grande échelle.

- **Cas d'usage typiques :**
    
	- **L'imagerie médicale :** Un hôpital possède 100 000 scanners, mais les médecins n'ont eu le temps d'en annoter manuellement que 1 000 ("Sain" ou "Malade"). L'algorithme utilise les 1 000 annotés pour apprendre les bases, puis exploite les 99 000 autres pour comprendre la structure générale des organes et améliorer la précision de ses futurs diagnostics.
    
    - **La reconnaissance vocale :** Transcrire manuellement des milliers d'heures d'enregistrements audio est fastidieux. On annote quelques heures, et le modèle utilise le reste des audios bruts pour s'améliorer.

---

## 5. Apprentissage par Renforcement (Reinforcement Learning)

**Le principe :** Un agent virtuel interagit avec un environnement. Il n'a pas de données historiques à analyser. Il apprend par essais et erreurs.

**L'objectif :** Trouver la meilleure stratégie pour maximiser son score de récompenses sur le long terme.

**Cas d'usage typiques :**
* Apprendre à une IA à jouer aux échecs ou à Mario Bros (la récompense est le score ou la victoire).
* La robotique (apprendre à un bras mécanique à attraper un objet sans le casser).
* La voiture autonome (récompense : rester sur la route ; pénalité : collision).

![Image_Apprentissage_Renforcement](Fiches-IA-SNN/Machine_Learning/00_Images/Pasted%20image%2020260611104410.png)