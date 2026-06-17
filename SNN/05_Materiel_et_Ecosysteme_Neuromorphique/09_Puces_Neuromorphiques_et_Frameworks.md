## 1. Le Goulot d'Étranglement de Von Neumann

Pourquoi ne pas simplement faire tourner des SNN sur des processeurs classiques (CPU) ou des cartes graphiques (GPU) ?

Les ordinateurs traditionnels utilisent l'architecture de **Von Neumann** : la mémoire (RAM) et le processeur (CPU) sont séparés physiquement. Pour faire un calcul, il faut constamment déplacer la donnée de la RAM vers le CPU, puis la renvoyer. Ce va-et-vient permanent consomme énormément d'énergie et de temps (c'est le goulot d'étranglement, ou _Von Neumann bottleneck_).

Le cerveau humain, lui, n'a pas de RAM séparée du CPU. Chaque synapse est **à la fois la mémoire et le processeur**. Les puces neuromorphiques tentent de reproduire cette architecture : la mémoire est physiquement distribuée à côté de chaque neurone artificiel.

## 2. Le Matériel : Les Puces Neuromorphiques

Plusieurs géants de la technologie et universités ont créé des puces (Hardware) spécifiquement conçues pour exécuter des SNN avec une efficacité énergétique redoutable.

- **Loihi (1 et 2) par Intel :** C'est la puce de recherche la plus avancée et la plus utilisée actuellement. Elle est entièrement numérique, supporte des modèles neuronaux complexes (comme l'ALIF), l'apprentissage local (STDP), et consomme jusqu'à 1000 fois moins d'énergie qu'un GPU pour la même tâche.

- **TrueNorth par IBM :** L'un des pionniers. Une puce massivement parallèle contenant 1 million de neurones. Très efficace pour des tâches de détection, mais moins flexible que Loihi (les poids synaptiques y sont contraints à des valeurs binaires ou ternaires).

- **SpiNNaker (Université de Manchester) :** Issu du _Human Brain Project_. Contrairement aux autres, il est basé sur des milliers de processeurs ARM classiques interconnectés par un réseau asynchrone ultra-rapide. Il vise la simulation de pans entiers de cerveaux biologiques à l'échelle du milliard de neurones.

- **BrainScaleS (Université d'Heidelberg) :** Une puce hybride analogique/numérique. La dynamique de la membrane (le condensateur) n'est pas calculée par un programme, mais simulée par de vrais composants électroniques physiques. Résultat : elle tourne jusqu'à 10 000 fois plus vite que le temps réel biologique.

## 3. Le Logiciel : Les Frameworks Python

Aujourd'hui, il n'est plus nécessaire d'écrire des équations différentielles en C++ pour coder un SNN. L'écosystème s'est standardisé autour de **Python** et s'appuie massivement sur PyTorch pour utiliser les _Surrogate Gradients_ (Fiche 05).

Les bibliothèques les plus populaires :

- **snnTorch :** Construit directement par-dessus PyTorch. Il est extrêmement didactique, très bien documenté et idéal pour intégrer des SNN dans des pipelines de Deep Learning classiques (BPTT, Surrogate Gradients).

- **SpikingJelly :** Un framework très complet et rapide (open-source, d'origine chinoise), qui supporte très bien l'intégration avec le matériel neuromorphique et le traitement des données événementielles (Caméras DVS).

- **Nengo :** Un framework d'un niveau d'abstraction plus élevé. Il utilise le NEF (Neural Engineering Framework) pour compiler des réseaux vers différentes puces physiques (Loihi, SpiNNaker) sans avoir à réécrire le code.

- **BindsNET :** Également basé sur PyTorch, mais orienté vers l'apprentissage biologiquement réaliste (STDP) et l'apprentissage par renforcement neuromorphique.

## 4. Le Workflow Standard en 2026

Aujourd'hui, le cycle de vie d'un projet SNN industriel (IA embarquée) ressemble à cela :

1. **Acquisition :** On capte des données "Sparses" avec une caméra événementielle (DVS).

2. **Entraînement Hors-ligne :** On construit un SNN via _snnTorch_ et on l'entraîne sur des serveurs surpuissants équipés de GPU Nvidia, en utilisant la BPTT et les _Surrogate Gradients_.

3. **Déploiement (Inférence) :** Une fois les poids synaptiques figés, le modèle est compilé et exporté.

4. **Exécution (Edge Computing) :** Le modèle tourne sur une petite puce neuromorphique (ex: Intel Loihi embarquée dans un drone). Il consomme quelques milliwatts et réagit en moins d'une milliseconde.