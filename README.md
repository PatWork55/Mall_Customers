# Segmentation de clients avec K-Means

## Présentation du projet

Ce projet a pour objectif de réaliser une segmentation de clients à partir du dataset **Mall Customers**.  
L'approche utilisée est le **clustering non supervisé**, plus précisément l'algorithme **K-Means**.

L'idée principale est d'identifier différents profils de clients à partir de leurs caractéristiques, notamment leur revenu annuel et leur score de dépense.

Ce projet permet de reprendre les principales étapes d'un projet Data :

- chargement des données ;
- analyse exploratoire ;
- visualisation ;
- préparation des variables ;
- normalisation ;
- choix du nombre de clusters ;
- entraînement d'un modèle de clustering ;
- interprétation métier des résultats ;
- évaluation avec le score de silhouette.

---

## Dataset utilisé

Le dataset utilisé est **Mall Customers**.

Il contient 200 clients et 5 colonnes :

| Colonne | Description |
|---|---|
| `CustomerID` | Identifiant unique du client |
| `Gender` | Genre du client |
| `Age` | Âge du client |
| `Annual_Income` | Revenu annuel du client, en milliers de dollars |
| `Spending_Score` | Score de dépense du client, de 1 à 100 |

La colonne `CustomerID` n'a pas été utilisée pour le clustering, car il s'agit uniquement d'un identifiant.

---

## Objectif métier

L'objectif est de segmenter les clients en groupes homogènes afin de mieux comprendre leurs comportements.

Une telle segmentation peut aider une entreprise à :

- identifier ses clients les plus intéressants ;
- adapter ses campagnes marketing ;
- proposer des offres personnalisées ;
- mieux comprendre les profils de consommation ;
- distinguer les clients à fort potentiel des clients moins engagés.

---

## Analyse exploratoire des données

Une première analyse des données a été réalisée afin de comprendre la structure du dataset.

Les principales observations sont les suivantes :

- le dataset contient 200 clients ;
- l'âge des clients varie de 18 à 70 ans ;
- le revenu annuel varie de 15k$ à 137k$ ;
- le score de dépense varie de 1 à 99 ;
- aucune variable cible n'est disponible, ce qui justifie l'utilisation d'une méthode non supervisée ;
- les variables `Annual_Income` et `Spending_Score` montrent visuellement des groupes intéressants dans un nuage de points.

L'analyse de corrélation a montré qu'il n'existe presque pas de corrélation linéaire entre `Annual_Income` et `Spending_Score`.

Cependant, l'absence de corrélation ne signifie pas absence de structure.  
La corrélation mesure une relation linéaire globale entre deux variables, tandis que le clustering cherche des groupes de points proches les uns des autres.

Dans ce projet, les données ne suivent pas une tendance linéaire simple, mais elles forment tout de même plusieurs groupes bien distincts.

---

## Préparation des données

Pour le premier modèle de clustering, deux variables ont été sélectionnées :

- `Annual_Income`
- `Spending_Score`

Ces variables ont été choisies parce qu'elles permettent de visualiser facilement les groupes de clients en deux dimensions.

Avant d'appliquer K-Means, les données ont été normalisées avec `StandardScaler`.

Cette étape est importante car K-Means est basé sur les distances.  
Sans normalisation, une variable ayant une échelle plus grande pourrait influencer davantage la formation des clusters.

---

## Choix du nombre de clusters

Le nombre de clusters a été choisi à l'aide de deux méthodes :

1. la méthode du coude ;
2. le score de silhouette.

### Méthode du coude

La méthode du coude consiste à tester plusieurs valeurs de `k` et à observer l'évolution de l'inertie.

Dans ce projet, l'inertie diminue fortement jusqu'à `k = 5`, puis la baisse devient beaucoup plus faible.

Cela indique que `k = 5` est un bon compromis entre simplicité du modèle et qualité de segmentation.

### Score de silhouette

Le score de silhouette a également été calculé pour plusieurs valeurs de `k`.

Le meilleur score est obtenu pour `k = 5`, avec une valeur d'environ :

```text
0.555
