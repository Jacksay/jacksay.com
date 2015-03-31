---
layout: post
title:  "Importer et exporter des données avec MongoDB "
date:   2014-08-26
categories: mongodb
description: Tutoriel / pense-bête pour l'import et l'export de données avec MongoDB.
tags: ["MongoDB", "Base de données"]
---

Cette article présente deux utilitaires en ligne de commande proposés par MongoDB pour gérer l'importation et l'exporation de données depuis une base de données MongoDB.

<!--more-->

# Exporter des données MongoDB

## Utilisation de mongoexport

L'utilitaire **mongoexport**, comme son nom semble le suggérer, permet de faire des export de données depuis une base de données MongoDB.

Dans la pratique, on utilisera les deux options (obligatoire) : 

 - `--db <base>`, `-d <base>` pour préciser la base de données
 - `--collection <collection>`, `--c <collection>` pour indiquer la collection à exporter

Par exemple, pour exporter depuis la base de donnée mabase la collection macollection, on utilisera la commande : 

```bash
mongoexport --db mabase --collection macollection
```

Par défaut, le résultat de l'export est affiché dans la sortie standard. L'option --out (ou --o) permet de spécifier le nom du fichier où sera enregistré le résultat de l'export.

```bash
mongoexport --db mabase --collection macollection --out fichier.json
```

On peut également filter les champs à export avec l'option --fields <champ1[,champ2]>, les champs doivent être séparés par une virgule.

```bash
mongoexport --db mabase
   --collection macollection
   --fields nom,prenom
```

## Options d'exportation avec mongoexport

Quelques options utiles

 - `--host <hostname><:host>`: Permet de préciser le serveur mongodb
 - `--port <port>` : Précise le port si ce dernier n'est pas standard (par défaut 27017)
 - `--username <username>` ou `-u <username>` : L'identifiant de connexion au serveur
 - `--password <password>` : Le mot de passe
 - `--dbpath <path>` : Emplacement des données (Accès concurrent avec mongod ?)
 - `--csv` permet d'exporter au format CSV

Pour le reste, un petit mongoexport `--help` vous listera l'ensemble des options disponibles. Sinon faites un tour sur la documentation officielle sur le site : <http://docs.mongodb.org/v2.2/reference/mongoimport/>

# Importer des données dans MongoDB

De l'autre côté du miroir, la commande `mongoimport`, va permettre d'importer des données dans une base de données MongoDB à partir d'une source (un format JSON par exemple).

On retrouve les même options que pour l'export, les principales étant  `--db` pour indiquer dans quelle base de données importer les données et `--collection` pour indiquer dans quelle collection :

```bash
mongoimport --db mabase --collection macollection --file mes-donnees.json
```

Une fois l'import terminé, vous verrez d'afficher un petit rapport récapitulant les opérations effectuées : 

```
connected to: 127.0.0.1
2014-08-26T22:42:38.001+0200        Progress: 7707762/8205849   93%
2014-08-26T22:42:38.002+0200            9200    3066/second
2014-08-26T22:42:38.096+0200 check 9 9778
2014-08-26T22:42:38.096+0200 imported 9778 objects
```

## Options d'importation avec mongoimport

Parmi les options intéressantes : 

 - `--drop` : Vide la collection avant d'importer
 - `--upsert` : Met à jour les données si elles existent (en se basant sur le champ _id) et insert les autres
 - `--upsertFields <champ1[,champ2]>` Permet de préciser les champs à importer

Pour avoir plus d'informations, je vous invite à consulter la documentation officielle : <http://docs.mongodb.org/v2.2/reference/mongoimport/>