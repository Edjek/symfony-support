# Symfony Security Guide

<br>

<center>
<img src="https://symfony.com/logos/symfony_black_03.png" width="100">
</center>

<br>

> Ce guide vous explique comment configurer l'authentification et la connexion dans une application Symfony.

## Sommaire

-   [1. Vérification de la Sécurité](#1-vérification-de-la-sécurité)
-   [2. Création d'un Utilisateur](#2-création-dun-utilisateur)
    -   [Explication](#explication)
-   [3. Configuration du Fichier de Sécurité](#3-configuration-du-fichier-de-sécurité)
-   [4. Création d'un Formulaire d'Inscription](#4-création-dun-formulaire-dinscription)
    -   [Explication](#explication-1)
-   [5. Création d'un Formulaire de Connexion](#5-création-dun-formulaire-de-connexion)
    -   [Explication](#explication-2)
-   [6. Test des Routes de Sécurité](#6-test-des-routes-de-sécurité)
-   [7. Ajout de Rôles et de Permissions](#7-ajout-de-rôles-et-de-permissions)
    -   [Modification des Permissions](#modification-des-permissions)
-   [8. Personnalisation des Formulaires et Vues](#8-personnalisation-des-formulaires-et-vues)
-   [9. Générer les droits d'accès sur les contrôleurs](#9-générer-les-droits-daccès-sur-les-contrôleurs)
    -   [Explication](#explication-3)
-   [10. Personnalisation des Messages](#10-personnalisation-des-messages)
    -   [Explication](#explication-4)
-   [Conclusion](#conclusion)

## Introduction

La sécurité est un aspect crucial de toute application web. Symfony fournit un système de sécurité robuste qui vous permet de gérer l'authentification, l'autorisation et la protection contre les attaques CSRF. Ce guide vous explique comment configurer l'authentification et la connexion dans une application Symfony.

## Vérification de la Sécurité

Il est important de vérifier régulièrement les vulnérabilités de sécurité dans vos dépendances.

```bash
symfony check:security
```

## Création d'un Utilisateur

Créez une entité utilisateur avec Symfony. Cela configurera également le système de sécurité pour gérer les utilisateurs.

```bash
symfony console make:user
```

### Explication

Cette commande vous posera des questions pour configurer l'entité utilisateur, comme le nom de la classe et les champs nécessaires (e.g., `email`, `password`). Par défaut, elle créera une classe `User` dans le répertoire `src/Entity`. Cette classe implémentera l'interface `UserInterface` de Symfony pour gérer l'authentification.

## Configuration du Fichier de Sécurité

Après avoir créé l'utilisateur, configurez le fichier `config/packages/security.yaml` pour définir les pare-feu et les encodages des mots de passe.

## Création d'un Formulaire d'Inscription

Générez un formulaire d'inscription pour permettre aux utilisateurs de créer un compte.

```bash
symfony console make:registration-form
```

### Explication

Cette commande crée un formulaire d'inscription, un contrôleur, et met à jour l'entité `User` pour gérer l'inscription. Elle ajoute également une route `/register`.
Mettre à jour la base de données pour ajouter les champs de l'utilisateur nouvellement créés.

## Création d'un Formulaire de Connexion

Générez un formulaire de connexion et configurez votre système de sécurité pour utiliser ce formulaire.

```bash
symfony console make:security:form-login
```

### Explication

Cette commande crée un contrôleur de connexion et les templates associés, ainsi que les routes nécessaires. Par défaut, elle configurera les routes `/login` et `/logout`.

## Test des Routes de Sécurité

Vérifiez que les routes de connexion, déconnexion et inscription fonctionnent correctement en accédant aux URLs suivantes :

-   **Connexion :** `/login`
-   **Déconnexion :** `/logout`
-   **Inscription :** `/register`

## Ajout de Rôles et de Permissions

Pour ajouter des rôles à vos utilisateurs, modifiez l'entité `User` :

```php
class User implements UserInterface
{
    // ...

    #[ORM\Column(type: 'json')]
    private array $roles = [];

    public function getRoles(): array
    {
        $roles = $this->roles;
        // guarantee every user at least has ROLE_USER
        $roles[] = 'ROLE_USER';

        return array_unique($roles);
    }

    public function setRoles(array $roles): self
    {
        $this->roles = $roles;

        return $this;
    }
}
```

### Modification des Permissions

Mettez à jour `security.yaml` pour ajouter des restrictions basées sur les rôles :

```yaml
security:
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
        - { path: ^/profile, roles: ROLE_USER }
```

## Personnalisation des Formulaires et Vues

Personnalisez les templates dans le répertoire `templates` pour correspondre à votre design.

```twig
{% extends 'base.html.twig' %}

{% block body %}
    <h1>Login</h1>

    {% if is_granted('IS_AUTHENTICATED_REMEMBERED') %}
        <div class="mb-3">
            You are logged in as {{ app.user.username }}, <a href="{{ path('app_logout') }}">Logout</a>
        </div>
    {% endif %}
{% endblock %}
```

## 9. Générer les droits d'accès sur les contrôleurs

Pour restreindre l'accès à certaines parties de votre application, vous pouvez ajouter des annotations de sécurité aux contrôleurs :

```php
#[Route('/admin')]
class AdminController extends AbstractController
{
    #[Route('/dashboard', name: 'admin_dashboard')]
    #[IsGranted('ROLE_ADMIN')]
    public function dashboard(): Response
    {
        // ...
        $this->denyAccessUnlessGranted('ROLE_ADMIN');
    }
}
```

### Explication

Dans cet exemple, la méthode `dashboard` du contrôleur `AdminController` nécessite le rôle `ROLE_ADMIN` pour y accéder. Si un utilisateur n'a pas ce rôle, il sera redirigé vers la page de connexion. Vous pouvez également utiliser :

-   `#[IsGranted('ROLE_USER')]`,
-   `#[IsGranted('IS_AUTHENTICATED_FULLY')]`,
-   `#[IsGranted('IS_AUTHENTICATED_REMEMBERED')]`,
-   `#[IsGranted('IS_AUTHENTICATED_ANONYMOUSLY')]`.

## Personnalisation des Messages

Personnalisez les messages en ajoutant des traductions dans le fichier `translations/security.fr.yaml` :

```yaml
Invalid credentials: 'Identifiants invalides.'
You have been logged in: 'Vous êtes connecté.'
You have been logged out: 'Vous êtes déconnecté.'
```

### Explication

Cela permet de personnaliser les messages affichés lors de la connexion, de la déconnexion et en cas d'erreur d'authentification.

## Conclusion

En suivant ce guide, vous pouvez configurer un système d'authentification robuste dans votre application Symfony. Assurez-vous de tester chaque étape et de personnaliser les formulaires et les vues selon vos besoins.

---

**🔗 Liens Utiles :**

-   [Symfony Security Documentation](https://symfony.com/doc/current/security.html)
-   [OpenClassRooms Symfony 7 Security](https://openclassrooms.com/fr/courses/8264046-construisez-un-site-web-a-laide-du-framework-symfony-7/8402604-definissez-les-utilisateurs)

---

[🏠 Retour au sommaire](#)
