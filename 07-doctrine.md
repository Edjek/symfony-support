# 🚀 [Symfony](https://symfony.com/) | Doctrine ORM : Manipuler des objets PHP comme s'ils étaient des lignes de base de données

<br>

<center>
<img src="https://symfony.com/logos/symfony_black_03.png" width="100">
</center>

<br>

Par **Rachid EDJEKOUANE ⭐️**

> Doctrine ORM est un ORM (Object-Relational Mapping) pour PHP qui fournit une solution de persistance pour les applications PHP. Il fournit des fonctionnalités de mapping objet-relationnel puissantes et flexibles pour les applications PHP.

## Sommaire

-   [1. Introduction](#1-introduction)
-   [2. Installation](#2-installation)
-   [3. Configuration](#3-configuration)
-   [4. Entités](#4-entités)
-   [5. Repository](#5-repository)
-   [6. Query Builder](#6-query-builder)
-   [7. Migration](#7-migration)

### 1. Introduction

Doctrine va vous permettre de manipuler des objets PHP comme s'ils étaient des lignes de base de données. Vous n'aurez plus à écrire des requêtes SQL pour récupérer ou modifier des données dans la base de données.

### 2. Installation

Pour installer Doctrine, exécutez la commande suivante :

```bash
composer require doctrine
```

### 3. Configuration

Pour configurer Doctrine, vous devez ajouter les informations de connexion à la base de données dans le fichier `.env` :

```env
# .env.local
DATABASE_URL=mysql://db_user:db_password@db_host:db_port/db_name
```

Ensuite, vous pouvez configurer Doctrine dans le fichier `config/packages/doctrine.yaml` :

Pour pouvoir utiliser le slug dans les URL, modifiez le fichier `config/packages/doctrine.yaml` pour mettre `auto_mapping` à `true` :

```yaml
doctrine:
    orm:
        controller_resolver:
            auto_mapping: true
```

### 4. Entités

Une entité est une classe PHP qui correspond à une table dans la base de données. Voici un exemple d'entité `Product` :

```php
<?php

namespace App\Entity;

use App\Repository\ProductRepository;
use Doctrine\DBAL\Types\Types;
use Doctrine\ORM\Mapping as ORM;
use Symfony\Bridge\Doctrine\Validator\Constraints\UniqueEntity;
use Symfony\Component\Validator\Constraints as Assert;


#[ORM\Entity(repositoryClass: ProductRepository::class)]
// Nous pouvons ajouter des méthodes de cycle de vie pour mettre à jour les dates de création et de mise à jour, en utilisant les annotations ORM
#[ORM\HasLifecycleCallbacks]
// Nous pouvons préciser que certaines propriétés de l'entité sont UNIQUE
#[UniqueEntity(fields: ['name'])]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column(type: 'integer')]
    private int $id;

    #[ORM\Column(length: 255)]
    // Nous pouvons ajouter des contraintes de validation
    #[Assert\NotBlank]
    #[Assert\Length(min: 3, max: 255)]
    private string $name;

    #[ORM\Column(type: 'decimal', precision: 10, scale: 2)]
    private float $price;

    #[ORM\Column(type: 'datetime_immutable')]
    private \DateTimeImmutable $createdAt;

    #[ORM\Column(type: 'datetime_immutable')]
    private \DateTimeImmutable $updatedAt;

    // Nous pouvons ajouter des méthodes de cycle de vie pour mettre à jour les dates de création et de mise à jour
    #[PrePersist]
    public function setCreatedAtValue(): void
    {
        $this->createdAt = new \DateTimeImmutable();
    }

    #[PreUpdate]
    public function setUpdatedAtValue(): void
    {
        $this->updatedAt = new \DateTimeImmutable();
    }

    // Getters et Setters
    // ...
```

### 5. Repository

Un repository est une classe qui permet de récupérer des entités à partir de la base de données. Voici un exemple de repository `ProductRepository` :

```php
<?php

namespace App\Repository;

use App\Entity\Product;
use Doctrine\Bundle\DoctrineBundle\Repository\ServiceEntityRepository;
use Doctrine\Persistence\ManagerRegistry;

// Le repository doit hériter de la classe ServiceEntityRepository, cela permet d'utiliser les méthodes de base de données de Doctrine ORM :
// find()
// findBy(array $criteria, array $orderBy = null, $limit = null, $offset = null)
// findOneBy(array $criteria, array $orderBy = null)
// findAll()
class ProductRepository extends ServiceEntityRepository
{
    public function __construct(ManagerRegistry $registry)
    {
        parent::__construct($registry, Product::class);
    }
}
```

### 6. Query Builder

Le Query Builder est une classe qui permet de construire des requêtes SQL de manière programmatique, les `Repository` heritent de la méthodes `createQueryBuilder()`. Voici un exemple de requête SQL pour récupérer tous les produits :

```php
<?php

public function findAllProducts(float $price): array
{
    return $this->createQueryBuilder('p')
        ->select('p.name', 'p.price')
        ->where('p.price > :price')
        ->orderBy('p.name', 'ASC')
        ->setParameter('price', $price)
        ->getQuery()
        ->getResult();
}
```

### 7. Migration

Les migrations permettent de mettre à jour la base de données en fonction des modifications apportées aux entités.

Pour créer une migration, exécutez la commande suivante :

```bash
php bin/console make:migration
```

Pour exécuter la migration, exécutez la commande suivante :

```bash
php bin/console doctrine:migrations:migrate
```

---

**🔗 Liens Utiles :**

-   [Doctrine ORM](https://www.doctrine-project.org/projects/orm.html)
-   [Openclassrooms - Doctrine](https://openclassrooms.com/fr/courses/3619856-developpez-votre-site-web-avec-le-framework-symfony/3622293-la-couche-metier-les-entites)

---

[🏠 Retour au sommaire](#)
