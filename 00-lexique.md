# Symfony

<br>

<center>
<img src="https://symfony.com/logos/symfony_black_03.png" width="100">
</center>

<br>

> Symfony est un framework PHP qui permet de développer des applications web. Il fournit des fonctionnalités de base telles que le moteur de templates, la gestion des formulaires, la gestion des traductions, etc. En outre, Symfony permet de développer des applications web plus complexes comme des API REST, des applications websockets, des applications web en temps réel, etc.

## Sommaire

-   [1. MVC (Model-View-Controller)](#1-mvc-model-view-controller)
-   [2. Arborescence d'un projet Symfony 7](#2-arborescence-dun-projet-symfony-7)
-   [3. Controller](#3-controller)
-   [4. Twig](#4-twig)
-   [5. Doctrine](#5-doctrine)
-   [6. Entity](#6-entity)
-   [7. Repository](#7-repository)
-   [8. Type ou FormType](#8-type-ou-formtype)
-   [9. YAML](#9-yaml)
-   [10. Service](#10-service)
-   [11. Commande](#11-commande)
-   [12. Fixtures](#12-fixtures)
-   [13. Event](#13-event)
-   [Références](#références)

## 1. MVC (Model-View-Controller)

    Le modèle MVC (Model-View-Controller) est un modèle d'architecture logicielle qui sépare les données, la logique et l'interface utilisateur d'une application. Il est composé de trois parties :

-   **Modèle** : représente la gestion de la base de données de l'application et contient la logique métier.

-   **Vue** : représente l'affichage des données de l'application. Elle est chargée de l'interface utilisateur (front).

-   **Contrôleur** : un controller est une classe PHP qui fait le lien entre le modèle et la vue. Il récupère les données du modèle, les traite et les transmet à la vue.

## 2. Arborescence d'un projet Symfony 7

```bash
assets/ # Contient les fichiers assets (images, css, js) non accessibles depuis le navigateur
bin/ # Contient les fichiers exécutables (console)
config/ # Contient les fichiers de configuration de Symfony
migrations/ # Contient les fichiers de migration de la base de données
public/ # Contient les fichiers publics (images, css, js) accessibles depuis le navigateur
src/ # Contient le code source de l'application
    Controller/ # Contient les contrôleurs de l'application (classes PHP)
    Entity/ # Contient les entités de l'application (représentation des tables de la base de données)
    Form/ # Contient les formulaires de l'application (FormType)
    Repository/ # Contient les dépôts de l'application (requêtes SQL)
templates/ # Contient les fichiers de templates de l'application (fichiers Twig)
tests/ # Contient les fichiers de tests de l'application
translations/ # Contient les fichiers de traduction de l'application
var/ # Contient les fichiers temporaires de l'application
vendor/ # Contient les dépendances de l'application
.env # Fichier de configuration de l'application
```

## 3. Controller

Les contrôleurs interprètent les requêtes HTTP effectuées via l'URL et renvoient les informations demandées par l'utilisateur à Twig, qui est la Vue.
