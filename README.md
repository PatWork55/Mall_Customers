# Segmentation de clients avec K-Means

## Présentation du projet

Ce projet a pour objectif de réaliser une segmentation de clients à partir du dataset **Mall Customers**.

L'approche utilisée est le **clustering non supervisé**, avec l'algorithme **K-Means**.

L'objectif est d'identifier différents groupes de clients selon leurs comportements, principalement à partir de deux variables :

- `Annual_Income` : revenu annuel du client ;
- `Spending_Score` : score de dépense du client.

Ce type d'analyse peut aider une entreprise à mieux comprendre sa clientèle et à adapter ses stratégies marketing.

---

## Dataset utilisé

Le dataset utilisé est **Mall Customers**.

Il contient **200 clients** et **5 colonnes** :

| Colonne | Description |
|---|---|
| `CustomerID` | Identifiant unique du client |
| `Gender` | Genre du client |
| `Age` | Âge du client |
| `Annual_Income` | Revenu annuel du client, en milliers de dollars |
| `Spending_Score` | Score de dépense du client, entre 1 et 100 |

La colonne `CustomerID` n'a pas été utilisée pour le clustering, car elle correspond seulement à un identifiant.

---

## Objectif du projet

L'objectif principal est de segmenter les clients en groupes homogènes afin de répondre à la question suivante :

> Peut-on identifier différents profils de clients à partir de leur revenu annuel et de leur score de dépense ?

Cette segmentation peut permettre de :

- mieux comprendre les comportements d'achat ;
- identifier les clients à forte valeur ;
- repérer les clients à fort potentiel ;
- adapter les campagnes marketing selon les profils ;
- proposer des offres plus personnalisées.

---

## Analyse exploratoire des données

Une première analyse exploratoire a été réalisée afin de comprendre la structure du dataset.

### Statistiques principales

| Variable | Minimum | Moyenne | Maximum |
|---|---:|---:|---:|
| `Age` | 18 | 38.85 | 70 |
| `Annual_Income` | 15 | 60.56 | 137 |
| `Spending_Score` | 1 | 50.20 | 99 |

Le dataset ne contient pas de valeurs manquantes.

Aucun doublon n'a été détecté.

---

## Analyse de corrélation

Une matrice de corrélation a été calculée sur les variables numériques utiles.

Les résultats principaux sont les suivants :

| Variables | Corrélation |
|---|---:|
| `Age` / `Annual_Income` | -0.012 |
| `Age` / `Spending_Score` | -0.327 |
| `Annual_Income` / `Spending_Score` | 0.010 |

La corrélation entre `Annual_Income` et `Spending_Score` est donc presque nulle.

Cependant, cela ne signifie pas qu'il n'existe pas de structure dans les données.  
La corrélation mesure une relation linéaire globale, tandis que le clustering cherche des groupes de points proches les uns des autres.

Dans ce projet, même si les variables ne sont pas corrélées linéairement, le nuage de points montre plusieurs groupes distincts.

---

## Variables utilisées pour le clustering

Pour le premier modèle de clustering, deux variables ont été sélectionnées :

| Variable | Rôle |
|---|---|
| `Annual_Income` | Représente le niveau de revenu du client |
| `Spending_Score` | Représente le comportement de dépense du client |

Ces deux variables ont été choisies parce qu'elles permettent de visualiser facilement les clusters en deux dimensions.

---

## Normalisation des données

Avant d'appliquer K-Means, les deux variables sélectionnées ont été normalisées avec `StandardScaler`.

Cette étape est importante, car K-Means est basé sur les distances.  
Sans normalisation, une variable avec une échelle plus grande pourrait avoir plus d'influence sur la formation des clusters.

Après normalisation, les variables ont une moyenne proche de `0` et un écart-type proche de `1`.

---

## Choix du nombre de clusters

Le nombre de clusters a été choisi à l'aide de deux méthodes :

1. la méthode du coude ;
2. le score de silhouette.

---

## Méthode du coude

La méthode du coude consiste à tester plusieurs valeurs de `k` et à observer l'évolution de l'inertie.

L'inertie mesure la compacité des clusters : plus elle est faible, plus les points sont proches de leur centre de cluster.

### Résultats de l'inertie

| k | Inertie |
|---:|---:|
| 1 | 400.000 |
| 2 | 269.691 |
| 3 | 157.704 |
| 4 | 108.921 |
| 5 | 65.568 |
| 6 | 55.057 |
| 7 | 44.865 |
| 8 | 37.228 |
| 9 | 32.392 |
| 10 | 29.982 |

La baisse de l'inertie est forte jusqu'à `k = 5`, puis elle devient beaucoup plus faible.

La méthode du coude suggère donc de choisir :

```text
k = 5
```

---

## Entraînement du modèle K-Means

Le modèle final a été entraîné avec les paramètres suivants :

```python
KMeans(n_clusters=5, random_state=42, n_init=10)
```

- `n_clusters=5` : nombre de clusters retenu ;
- `random_state=42` : permet d'avoir des résultats reproductibles ;
- `n_init=10` : l'algorithme est lancé plusieurs fois avec différentes initialisations, puis conserve le meilleur résultat.

---

## Résultats finaux du clustering

Le modèle K-Means a identifié **5 segments de clients**.

### Répartition des clients par cluster

| Cluster | Nombre de clients |
|---:|---:|
| 0 | 81 |
| 1 | 39 |
| 2 | 22 |
| 3 | 35 |
| 4 | 23 |

Le cluster le plus important est le **cluster 0**, avec 81 clients.

---

## Profil moyen des clusters

| Cluster | Âge moyen | Revenu annuel moyen | Score de dépense moyen | Nombre de clients |
|---:|---:|---:|---:|---:|
| 0 | 42.72 | 55.30 | 49.52 | 81 |
| 1 | 32.69 | 86.54 | 82.13 | 39 |
| 2 | 25.27 | 25.73 | 79.36 | 22 |
| 3 | 41.11 | 88.20 | 17.11 | 35 |
| 4 | 45.22 | 26.30 | 20.91 | 23 |

L'âge n'a pas été utilisé pour construire les clusters.  
Il est utilisé après coup pour mieux interpréter les profils obtenus.

---

## Centroïdes des clusters

Les centroïdes représentent les centres moyens des clusters.

| Cluster | Revenu annuel moyen du centroïde | Score de dépense moyen du centroïde |
|---:|---:|---:|
| 0 | 55.30 | 49.52 |
| 1 | 86.54 | 82.13 |
| 2 | 25.73 | 79.36 |
| 3 | 88.20 | 17.11 |
| 4 | 26.30 | 20.91 |

Ces centroïdes confirment que les clusters sont principalement structurés autour de deux dimensions :

- niveau de revenu ;
- niveau de dépense.

---

## Interprétation métier des clusters

Les clusters ont été renommés afin de faciliter leur interprétation métier.

| Cluster | Nom du segment | Description |
|---:|---|---|
| 0 | Clients standards / équilibrés | Clients avec un revenu moyen et un score de dépense moyen |
| 1 | Clients premium à forte valeur | Clients avec un revenu élevé et un score de dépense élevé |
| 2 | Jeunes dépensiers à petit revenu | Clients plutôt jeunes, avec un faible revenu mais un score de dépense élevé |
| 3 | Clients aisés mais prudents | Clients avec un revenu élevé mais un faible score de dépense |
| 4 | Clients économes à faible revenu | Clients avec un faible revenu et un faible score de dépense |

---

## Analyse détaillée des segments

### Cluster 0 — Clients standards / équilibrés

Ce groupe contient **81 clients**.

Ces clients ont :

- un âge moyen de 42.72 ans ;
- un revenu annuel moyen de 55.30k$ ;
- un score de dépense moyen de 49.52.

Ils représentent le segment le plus important du dataset.  
Ce sont des clients avec un comportement moyen, sans profil extrême.

Ils peuvent être considérés comme une base stable de clientèle.

---

### Cluster 1 — Clients premium à forte valeur

Ce groupe contient **39 clients**.

Ces clients ont :

- un âge moyen de 32.69 ans ;
- un revenu annuel moyen de 86.54k$ ;
- un score de dépense moyen de 82.13.

Ce segment est très intéressant d'un point de vue marketing.  
Ces clients ont à la fois un revenu élevé et une forte tendance à dépenser.

Ils peuvent être ciblés avec :

- des offres premium ;
- des programmes de fidélité ;
- des services personnalisés ;
- des avantages exclusifs.

---

### Cluster 2 — Jeunes dépensiers à petit revenu

Ce groupe contient **22 clients**.

Ces clients ont :

- un âge moyen de 25.27 ans ;
- un revenu annuel moyen de 25.73k$ ;
- un score de dépense moyen de 79.36.

Ce segment regroupe des clients plutôt jeunes qui dépensent beaucoup malgré un revenu faible.

Ils peuvent être intéressants pour :

- des offres accessibles ;
- des promotions ;
- des campagnes orientées jeunes consommateurs ;
- des produits tendance ou attractifs.

---

### Cluster 3 — Clients aisés mais prudents

Ce groupe contient **35 clients**.

Ces clients ont :

- un âge moyen de 41.11 ans ;
- un revenu annuel moyen de 88.20k$ ;
- un score de dépense moyen de 17.11.

Ce segment dispose d'un revenu élevé, mais dépense peu.

Ces clients représentent un potentiel commercial important, mais ils semblent moins engagés.

Ils pourraient être ciblés avec :

- des campagnes personnalisées ;
- des offres de découverte ;
- des recommandations adaptées ;
- des actions visant à comprendre leurs freins à l'achat.

---

### Cluster 4 — Clients économes à faible revenu

Ce groupe contient **23 clients**.

Ces clients ont :

- un âge moyen de 45.22 ans ;
- un revenu annuel moyen de 26.30k$ ;
- un score de dépense moyen de 20.91.

Ce segment regroupe des clients à faible revenu et faible score de dépense.

Ils peuvent être ciblés avec :

- des offres économiques ;
- des réductions ;
- des promotions adaptées ;
- des produits d'entrée de gamme.

---

## Évaluation avec le score de silhouette

Le score de silhouette permet d'évaluer la qualité d'un clustering.

Il mesure si les points sont proches des autres points de leur cluster et éloignés des points des autres clusters.

Le score varie entre `-1` et `1` :

| Score | Interprétation |
|---|---|
| Proche de 1 | Clusters bien séparés |
| Proche de 0 | Clusters qui se chevauchent |
| Proche de -1 | Mauvaise affectation des points |

---

## Résultats du score de silhouette

Les scores de silhouette ont été calculés pour plusieurs valeurs de `k`.

| k | Score de silhouette |
|---:|---:|
| 2 | 0.321 |
| 3 | 0.467 |
| 4 | 0.494 |
| 5 | 0.555 |
| 6 | 0.540 |
| 7 | 0.528 |
| 8 | 0.455 |
| 9 | 0.457 |
| 10 | 0.443 |

Le meilleur score est obtenu pour :

```text
k = 5
```

Avec un score de silhouette de :

```text
0.555
```

Ce résultat confirme le choix obtenu avec la méthode du coude.

---

## Conclusion sur le choix de k

Les deux méthodes utilisées donnent un résultat cohérent :

| Méthode | Résultat |
|---|---|
| Méthode du coude | k = 5 |
| Score de silhouette | k = 5 |

Le choix final retenu est donc :

```text
k = 5
```

Cela signifie que la segmentation finale contient cinq groupes de clients.

---

## Technologies utilisées

Le projet a été réalisé avec Python et les bibliothèques suivantes :

| Bibliothèque | Utilisation |
|---|---|
| `pandas` | Chargement, manipulation et analyse des données |
| `matplotlib` | Visualisation des données |
| `scikit-learn` | Normalisation, K-Means et score de silhouette |

---

## Structure du projet

```text
.
├── Mall_Customers.ipynb
└── README.md
```

---

## Comment exécuter le projet

1. Cloner le dépôt :

```bash
git clone <url-du-repo>
```

2. Ouvrir le notebook :

```text
Mall_Customers.ipynb
```

3. Exécuter les cellules dans l'ordre.

Le notebook contient toutes les étapes du projet :

- chargement des données ;
- analyse exploratoire ;
- normalisation ;
- entraînement du modèle ;
- évaluation ;
- interprétation des résultats.

---

## Résumé final

Ce projet a permis de construire une segmentation de clients avec l'algorithme K-Means.

Le dataset contient 200 clients et plusieurs variables descriptives.  
Après analyse exploratoire, les variables `Annual_Income` et `Spending_Score` ont été retenues pour réaliser le clustering.

Même si ces deux variables ne sont presque pas corrélées linéairement, elles permettent de former des groupes bien distincts.  
Cela montre que l'absence de corrélation ne signifie pas forcément absence de structure.

La méthode du coude et le score de silhouette ont tous deux confirmé que le meilleur choix était de retenir **5 clusters**.

Le modèle final permet d'identifier cinq profils clients :

1. clients standards / équilibrés ;
2. clients premium à forte valeur ;
3. jeunes dépensiers à petit revenu ;
4. clients aisés mais prudents ;
5. clients économes à faible revenu.

Le score de silhouette final est d'environ **0.555**, ce qui indique une qualité de clustering correcte pour un modèle simple utilisant seulement deux variables.

Cette segmentation peut servir de base à des décisions marketing, notamment pour adapter les offres, cibler les bons profils et mieux comprendre les comportements d'achat.

---

## Limites du projet

Cette analyse présente plusieurs limites :

- seules deux variables ont été utilisées pour construire les clusters ;
- l'âge et le genre n'ont pas été intégrés dans le modèle final ;
- les noms donnés aux clusters sont des interprétations métier ;
- K-Means suppose que les clusters sont relativement compacts ;
- les résultats pourraient changer avec d'autres variables ou d'autres algorithmes.

---

## Pistes d'amélioration

Pour aller plus loin, il serait possible de :

- intégrer l'âge dans le clustering ;
- encoder la variable `Gender` ;
- comparer K-Means avec DBSCAN ;
- comparer K-Means avec le clustering hiérarchique ;
- créer des visualisations plus professionnelles ;
- sauvegarder les graphiques dans un dossier `images/` pour les afficher directement dans le README ;
- construire des recommandations marketing détaillées pour chaque segment.

---

## Conclusion générale

Ce projet montre comment utiliser le clustering pour analyser une base de clients sans variable cible.

K-Means a permis d'identifier cinq segments distincts à partir du revenu annuel et du score de dépense.  
Les résultats obtenus sont cohérents, interprétables et exploitables d'un point de vue métier.

Le projet constitue donc une bonne introduction à l'apprentissage non supervisé et à la segmentation client.
