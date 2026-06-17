# Base de Connaissances : Machine Learning, Deep Learning & SNN

> **Dépôt de Synthèse — Stage de Recherche de Licence 3** Un parcours complet depuis les fondations mathématiques de l'IA classique jusqu'aux frontières de l'informatique neuromorphique.

## 🗺️ Feuille de Route & Progression Logique

Cette cartographie conceptuelle retrace l'architecture exacte du dépôt. Elle associe chaque répertoire physique (chapitre) aux fiches de synthèse (fichiers) associées, en plaçant la préparation et le nettoyage des données en amont de toute phase de modélisation :

```
┌────────────────────────────────────────────────────────────────────────┐
│ 📂 Machine_Learning/01_Fondations/                                     │
│ └─► [01_Types_Apprentissage] ──► [02_Cycle_De_Vie] ──► [03_Vocab]      │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 📂 Machine_Learning/02_Preparation_Donnees/                            │
│ └─► [04_Nettoyage_Encodage] ──► [05_Mise_A_L_Echelle]                  │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 📂 APPRENTISSAGE CLASSIQUE (ML)                                        │
│                                                                        │
│  📂 Machine_Learning/03_Apprentissage_Supervise/                       │
│  └─► [06_Regression] ──► [07_Classification] ──► [08_Methodes_Ensemble]│
│                                                                        │
│  📂 Machine_Learning/04_Apprentissage_Non_Supervise/                   │
│  └─► [09_Clustering] ──► [10_Reduction_Dimensionnalite]                │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 📂 Machine_Learning/05_Optimisation_Modele/                            │
│ └─► [11_Strategies_Validation] ──► [12_Hyperparametres_Regularisation] │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│ 📂 Machine_Learning/06_Deep_Learning/                                  │
│ └─► [13_Vocab] ──► [14_Propagation] ──► [15_App_Profond]               │
│ ──► [16_CNN_Specialise] ──► [17_RNN_LSTM] ──► [18_Non_Sup]             │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼ 
(Attention & Contexte)					              (Le Pont Biologique)
┌────────────────────────────────────────┐ ┌────────────────────────────────────┐
│ 📂 Machine_Learning/07_Transformers/   │ │ 📂 SNN/01_Fondations_Bio_Math/     │
│ ├─► [19_Embeddings_Tokens]             │ │ └─► [01_Action_Pot] ──► [02_LIF]   │
│ ├─► [20_Positional_Encoding]           │ │                                    │
│ ├─► [21_Self_Attention_QKV]            │ │ 📂 SNN/02_Traitement_Information/  │
│ ├─► [22_Softmax_et_Multi_Head]         │ │ └─► [03_Spatial_Temp] ──► [04_Impl]│
│ ├─► [23_Feed_Forward_et_Residuals]     │ │                                    │
│ └─► [24_Architecture_Globale]          │ │ 📂 SNN/03_App_Bio_Non_Supervise/   │
└────────────────────────────────────────┘ │ └─► [05_STDP]                      │
                                           │                                    │
                                           │ 📂 SNN/04_App_Profond_Supervise/   │
                                           │ └─► [06_Surrogate_BPT]──►[07_Conv] │
                                           │                                    │
                                           │ 📂 SNN/05_Hardware_Neuromorphique/ │
                                           │ └─► [08_DVS]──►[09_Puces_Framework]│
	                                       └────────────────────────────────────┘
```

## 📂 Organisation du Répertoire

### Partie 1 : Machine Learning & Deep Learning

#### 🟥 01 | Fondations & Concepts Clés

- [**`01_Types_Apprentissage.md`**](01_Types_Apprentissage.md "null") : IA, ML, DL et grands paradigmes (Supervisé, Non Supervisé, Renforcement).
    
- [**`02_Cycle_De_Vie.md`**](02_Cycle_De_Vie.md "null") : Cycle de vie d'un projet, de l'ingestion à la mise en production.
    
- [**`03_Vocabulaire_Essentiel.md`**](03_Vocabulaire_Essentiel.md "null") : Glossaire et définitions mathématiques fondamentales du ML.
    

#### 🟧 02 | Préparation des Données

- [**`04_Nettoyage_Encodage.md`**](04_Nettoyage_Encodage.md "null") : Imputation de données manquantes, gestion des outliers et encodages nominaux/ordinaux.
    
- [**`05_Mise_A_L_Echelle.md`**](05_Mise_A_L_Echelle.md "null") : Mise à l'échelle (Min-Max Scaling, Standardisation Z-Score, Robust Scaling).
    

#### 🟨 03 | Apprentissage Supervisé

- [**`06_Regression.md`**](06_Regression.md "null") : Régression linéaire, calcul d'erreur (MSE) et descente de gradient.
    
- [**`07_Classification.md`**](07_Classification.md "null") : Régression logistique, métriques de performance et de confusion.
    
- [**`08_Methodes_Ensemble.md`**](08_Methodes_Ensemble.md "null") : Algorithmes d'ensemble, Bagging (Random Forest) et Boosting.
    

#### 🟨 04 | Apprentissage Non Supervisé

- [**`09_Clustering.md`**](09_Clustering.md "null") : Regroupements par densité et distance (K-Means, DBSCAN).
    
- [**`10_Reduction_Dimensionnalite.md`**](10_Reduction_Dimensionnalite.md "null") : Projection et réduction de dimensionnalité (PCA).
    

#### 🟩 05 | Optimisation du Modèle

- [**`11_Strategies_Validation.md`**](11_Strategies_Validation.md "null") : Overfitting, Underfitting, validation croisée (K-Fold) et compromis biais-variance.
    
- [**`12_Hyperparametres_Regularisation.md`**](12_Hyperparametres_Regularisation.md "null") : Grid/Random Search, régularisation L1 (Lasso) et L2 (Ridge).
    

#### 🟦 06 | Deep Learning & Architectures Spécialisées

- [**`13_Vocabulaire_Deep_Learning.md`**](13_Vocabulaire_Deep_Learning.md "null") : Lexique technique (fonctions d'activation, fonctions de coût, optimiseurs).
    
- [**`14_Architectures_Propogation.md`**](14_Architectures_Propogation.md "null") : Modélisation mathématique du neurone artificiel (Perceptron) et Forward Pass.
    
- [**`15_Apprentissage_Profond.md`**](15_Apprentissage_Profond.md "null") : Rétropropagation du gradient (Backpropagation) et mise à jour des poids.
    
- [**`16_Architecture_Specialisees_CNN.md`**](16_Architecture_Specialisees_CNN.md "null") : Convolutions, Pooling et extraction de caractéristiques spatiales.
    
- [**`17_Architecture_Specialisees_RNN_LSTM.md`**](17_Architecture_Specialisees_RNN_LSTM.md "null") : Traitement de séquences temporelles et mécanismes de portes (LSTM, GRU).
    
- [**`18_Deep_Learning_Non_Supervise.md`**](18_Deep_Learning_Non_Supervise.md "null") : Autoencodeurs pour la compression et GANs (Generative Adversarial Networks).
    

#### 🟪 07 | Architectures Transformers

- [**`19_Embeddings_Tokens.md`**](19_Embeddings_Tokens.md "null") : Tokenisation sémantique et plongements lexicaux statiques.
    
- [**`20_Positional_Encoding.md`**](20_Positional_Encoding.md "null") : Encodage trigonométrique de l'ordre séquentiel sans récurrence.
    
- [**`21_Self_Attention_QKV.md`**](21_Self_Attention_QKV.md "null") : Alignement mathématique par produit scalaire (Queries, Keys, Values).
    
- [**`22_Softmax_et_Multi_Head.md`**](22_Softmax_et_Multi_Head.md "null") : Division en sous-espaces sémantiques et parallélisation de l'attention.
    
- [**`23_Feed_Forward_et_Residuals.md`**](23_Feed_Forward_et_Residuals.md "null") : Connexions résiduelles, LayerNorm et réseaux Feed-Forward (MLP).
    
- [**`24_Architecture_Globale.md`**](24_Architecture_Globale.md "null") : Synthèse et comparaison des modèles Encodeurs (BERT) et Décodeurs (GPT).
    

### Partie 2 : SNN (Spiking Neural Networks)

#### 🧠 01 | Fondations Neuromorphiques

- [**`01_Introduction_et_Potentiel_d_Action.md`**](01_Introduction_et_Potentiel_d_Action.md "null") : Biophysique de la membrane neuronale, canaux ioniques et potentiel d'action.
    
- [**`02_Modeles_de_Neurones_LIF.md`**](02_Modeles_de_Neurones_LIF.md "null") : Équations différentielles et modélisation électrique du neurone Leaky Integrate-and-Fire.
    

#### 🎛️ 02 | Traitement & Encodage

- [**`03_Encodage_Spatial_et_Temporel.md`**](03_Encodage_Spatial_et_Temporel.md "null") : Théorie du codage par fréquence (Rate Coding) et codage temporel fin.
    
- [**`04_Implementation_Encodage.md`**](04_Implementation_Encodage.md "null") : Génération de trains de spikes par processus de Poisson et gestion des tenseurs d'images RGB.
    

#### ⚡ 03 | Apprentissage Biologique

- [**`05_Plasticite_Synaptique_et_STDP.md`**](05_Plasticite_Synaptique_et_STDP.md "null") : Règle de Hebb et plasticité synaptique asymétrique dépendante du temps (STDP).
    

#### 🧮 04 | Apprentissage Profond Supervisé SNN

- [**`06_Surrogate_Gradients_et_BPTT.md`**](06_Surrogate_Gradients_et_BPTT.md "null") : Rétropropagation à travers le temps (BPTT) et gradients de substitution face au seuil non dérivable.
    
- [**`07_Conversion_ANN_vers_SNN.md`**](07_Conversion_ANN_vers_SNN.md "null") : Algorithme de portage de réseaux convolutifs classiques (ReLU) vers des puces neuromorphiques (IF).
    

#### 🔌 05 | Écosystème Hardware

- [**`08_Capteurs_Evenementiels_DVS.md`**](08_Capteurs_Evenementiels_DVS.md "null") : Caméras événementielles (DVS) et représentation asynchrone aspatiale de la vision (AER).
    
- [**`09_Puces_Neuromorphiques_et_Frameworks.md`**](09_Puces_Neuromorphiques_et_Frameworks.md "null") : Architecture de Von Neumann vs Neuromorphique (Intel Loihi), et prise en main de snnTorch.
    

## 🛠️ Utilisation de la Base de Connaissances

1. **Fiches théoriques :** Chaque document `.md` est autonome et contient des formules mathématiques écrites en LaTeX pour une lecture fluide (avec des délimiteurs `$` pour l'inline et `$$` pour les blocs complexes).
    
2. **Schémas et visuels :** Les fiches font référence à des illustrations stockées dans les dossiers respectifs `00_Images` afin de faciliter la mémoire visuelle.
    
3. **Progression :** Il est fortement recommandé de lire la base dans l'ordre numérique des dossiers pour bien assimiler la construction mathématique qui mène aux modèles modernes.