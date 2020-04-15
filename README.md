# ORM / API - Ynov B2 - Partiel

Vous allez réaliser une API de librairie musicale.

Critères d'évaluation :

- Organisation et séparation du code
- Création et configuration d'une API avec API Platform
- Application de filtres
- Utilisation des groupes de sérialisation
- Relations entre entités
- Authentification JWT
- DTO

## Mode de rendu

Lien vers votre dépôt Git, que je pourrai clôner.

## Dépôt Git

Votre dépôt Git contiendra un fichier `README.md` dans lequel vous indiquerez :

- Qu'est-ce qu'un DTO et à quoi sert-il ?
- Quelle est la différence entre un listener et un subscriber dans Symfony ?
- Qu'est-ce qu'un JWT ? Pourquoi l'utilise-t-on plutôt que les sessions PHP ?
- Qu'est-ce que CORS ?
- Quelle est la différence entre JSON et JSON-LD ?

## Exercice

Votre API contiendra des artistes, des styles, des albums et des utilisateurs.

Vous utiliserez API Platform pour réaliser cette API.

Le but de l'API est de pouvoir enregistrer de nouveaux styles, artistes, albums, etc...et de pouvoir mettre un artiste en "favori" pour un utilisateur.

>Votre API sera protégée derrière une authentification pour toutes les requêtes commençant par `/api`

### Structure de la base de données

> Entité : User

Entité qui servira à l'authentification.

Elle contiendra en plus :

- la liste des artistes que l'utilisateur a ajoutés en favori (attribut `artists`)
- un champ date `lastConnection` qui contiendra la date de dernière connexion de l'utilisateur. Ce champ sera donc à mettre à jour à chaque fois qu'un utilisateur se connecte avec succès
- un champ integer `failedAuth` contenant le nombre de tentatives de connexions échouées. Ce champ sera par défaut à `0` et devra être incrémenté de `1` à chaque fois qu'une tentative de connexion pour un utilisateur aura échoué (si l'adresse email renseignée est donc bonne, mais que le mot de passe est quant à lui incorrect)

> Entité : Artist

| Champ | Type | Commentaires |
|---|---|---|
| id | integer | Clé primaire |
| name | string | |
| startYear | integer | Année à laquelle l'artiste a débuté sa carrière |
| styles | **Relation** | Les styles associés à l'artiste |
| albums | **Relation** | Les albums que l'artiste a sortis |
| users | **Relation** | Les utilisateurs ayant mis l'artiste en favori |
| created | date | Date de création de l'enregistrement, renseignée automatiquement à la création de l'enregistrement |

> Entité : Style

| Champ | Type | Commentaires |
|---|---|---|
| id | integer | Clé primaire |
| name | string | |
| artists | **Relation** | Les artistes associés au style de musique |
| created | date | Date de création de l'enregistrement, renseignée automatiquement à la création de l'enregistrement |

> Entité : Album

| Champ | Type | Commentaires |
|---|---|---|
| id | integer | Clé primaire |
| name | string | |
| releaseYear | integer | Année de sortie de l'album |
| artist | **Relation** | Artiste qui a sorti l'album |
| created | date | Date de création de l'enregistrement, renseignée automatiquement à la création de l'enregistrement |

#### Vous réaliserez des fixtures pour chaque entité, afin d'avoir un jeu de données aléatoires initial

### Fonctionnalités

Toutes les entités seront des ressources de l'API présentant un CRUD complet.

#### Recherche

- Rechercher un album par son nom (stratégie : nom partiel, insensible à la casse)
- Rechercher un album par année de sortie (exacte, après telle année, ou entre telle et telle année)
- Rechercher un artiste par son nom (stratégie : nom partiel, insensible à la casse)
- Rechercher un artiste par le nom du style (stratégie : nom partiel, insensible à la casse)
- Rechercher un style par son nom (stratégie : commence par, insensible à la casse)

#### Inscription

Vous réaliserez un endpoint **publique** à part pour supporter l'inscription d'utilisateurs, dans un contrôleur.

Le contrôleur mettra en oeuvre un formulaire vérifiant un champ "Mot de passe" et un autre "Confirmation de mot de passe" qui doit être égal au champ "Mot de passe" pour valider l'enregistrement d'un utilisateur.

Une bonne gestion d'erreurs devra être mise en place afin de pouvoir renvoyer au client une réponse correctement formatée identifiant clairement l'erreur rencontrée, s'il y a une erreur bien sûr.

> N'oubliez pas de désactiver la protection CSRF dans le formulaire (voir mon post dans Discord, channel #php, du 14/04 à 12h34)

#### Formats

Les formats activés seront JSON-LD, JSON et CSV.

#### Contrôle d'accès

Les endpoints de la ressource `User` seront accessibles uniquement aux utilisateurs ayant le rôle `ROLE_ADMIN`.

#### Sérialisation & DTO

Les informations d'un artiste devront inclure :

- ID
- Nom
- Année de début de carrière
- Nombre d'albums sortis
- Nombre de fans (utilisateurs ayant ajouté l'artiste en favori)

Les informations d'un utilisateur devront inclure :

- ID
- email
- Nombre d'artistes favoris
