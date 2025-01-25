# 🚀 [Symfony](https://symfony.com/) | Mise en Production : Créer un environnement de production

<br>

<center>
<img src="https://symfony.com/logos/symfony_black_03.png" width="100">
</center>

<br>

Par **Rachid EDJEKOUANE ⭐️**

> Ce guide vous explique comment mettre en production une application Symfony pour la première fois.

## Sommaire

-   [Introduction](#introduction)
-   [Mise en Production](#mise-en-production)
    -   [1. Optimisez l'autoloader Composer :](#1-optimisez-lautoloader-composer-)
    -   [2. Videz le cache en mode production :](#2-videz-le-cache-en-mode-production-)
    -   [3. Réchauffez le cache :](#3-réchauffez-le-cache-)
    -   [4. Assurez-vous que les variables d'environnement sont correctement configurées pour la production dans le fichier `.env.local` ou via les variables d'environnement du serveur.](#4-assurez-vous-que-les-variables-denvironnement-sont-correctement-configurées-pour-la-production-dans-le-fichier-envlocal-ou-via-les-variables-denvironnement-du-serveur)
    -   [5. Désactivez le mode debug et le profiler dans votre fichier `.env` ou `.env.local` :](#5-désactivez-le-mode-debug-et-le-profiler-dans-votre-fichier-env-ou-envlocal-)
    -   [6. Si vous utilisez Doctrine, assurez-vous que votre schéma de base de données est à jour :](#6-si-vous-utilisez-doctrine-assurez-vous-que-votre-schéma-de-base-de-données-est-à-jour-)
    -   [7. Vérifiez que toutes les dépendances de production sont installées et que les dépendances de développement sont exclues :](#7-vérifiez-que-toutes-les-dépendances-de-production-sont-installées-et-que-les-dépendances-de-développement-sont-exclues-)
    -   [8. Configurez votre serveur web (Apache, Nginx, etc.) pour pointer vers le dossier `public/` de votre projet.](#8-configurez-votre-serveur-web-apache-nginx-etc-pour-pointer-vers-le-dossier-public-de-votre-projet)
    -   [9. Assurez-vous que les permissions des fichiers et dossiers sont correctement définies pour l'utilisateur du serveur web.](#9-assurez-vous-que-les-permissions-des-fichiers-et-dossiers-sont-correctement-définies-pour-lutilisateur-du-serveur-web)
    -   [10. Testez soigneusement votre application en mode production pour vous assurer que tout fonctionne comme prévu.](#10-testez-soigneusement-votre-application-en-mode-production-pour-vous-assurer-que-tout-fonctionne-comme-prévu)
-   [Conclusion](#conclusion)

---

### Introduction

Symfony est un framework PHP qui vous permet de créer des applications web robustes et évolutives. Il fournit un ensemble de composants et de bibliothèques qui facilitent le développement d'applications web modernes. Ce guide vous explique comment mettre en production une application Symfony pour la première fois.

### Mise en Production

Pour passer un projet Symfony 7 de l'environnement de développement (dev) à l'environnement de production (prod), voici les principales étapes à suivre :

#### 1. Optimisez l'autoloader Composer :

```bash
composer dump-autoload --optimize --no-dev --classmap-authoritative
```

Cela permet de générer un fichier d'autoload plus rapide et plus efficace pour les classes de votre application.

#### 2. Videz le cache en mode production :

```bash
php bin/console cache:clear --env=prod --no-debug
```

Cela permet de vider le cache de l'application et de forcer Symfony à recompiler les fichiers de cache en mode production.

#### 3. Réchauffez le cache :

```bash
php bin/console cache:warmup --env=prod --no-debug
```

Cela permet de précharger les fichiers de cache pour améliorer les performances de l'application.

#### 4. Assurez-vous que les variables d'environnement sont correctement configurées pour la production dans le fichier `.env.local` ou via les variables d'environnement du serveur.

#### 5. Désactivez le mode debug et le profiler dans votre fichier `.env` ou `.env.local` :

```bash
APP_ENV=prod
APP_DEBUG=0
```

Cela permet de désactiver le mode debug et le profiler Symfony en mode production.

#### 6. Si vous utilisez Doctrine, assurez-vous que votre schéma de base de données est à jour :

```bash
php bin/console doctrine:migrations:migrate --env=prod
```

Cela permet de mettre à jour la base de données en mode production.

#### 7. Vérifiez que toutes les dépendances de production sont installées et que les dépendances de développement sont exclues :

```bash
composer install --no-dev --optimize-autoloader
```

Cela permet de désactiver les dépendances de développement et de les installer en mode production.

#### 8. Configurez votre serveur web (Apache, Nginx, etc.) pour pointer vers le dossier `public/` de votre projet.

#### 9. Assurez-vous que les permissions des fichiers et dossiers sont correctement définies pour l'utilisateur du serveur web.

#### 10. Testez soigneusement votre application en mode production pour vous assurer que tout fonctionne comme prévu.

### Conclusion

En suivant ces étapes, vous passerez votre application Symfony 7 de l'environnement de développement à l'environnement de production de manière sécurisée et optimisée pour les performances. N'oubliez pas de tester soigneusement votre application en mode production pour vous assurer qu'elle fonctionne correctement avant de la mettre en ligne.

---

[🏠 Retour au sommaire](#)
