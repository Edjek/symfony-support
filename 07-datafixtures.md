# Fixtures

> Pour peupler la base de données avec des données de test, Symfony propose un outil appelé les fixtures. Les fixtures sont des classes PHP qui permettent de générer des données de test pour les entités de l'application.

## Sommaire

-   [Installation](#installation)
-   [Création des fixtures](#création-des-fixtures)
-   [Chargement des fixtures](#chargement-des-fixtures)
-   [Faker](#faker)
-   [Conclusion](#conclusion)

## Introduction

Les fixtures sont des données de test qui sont utilisées pour peupler la base de données. Elles sont généralement utilisées pour les tests unitaires et fonctionnels.

## Installation

Pour installer les fixtures, il faut ajouter le package `doctrine/doctrine-fixtures-bundle` à votre projet :

```bash
composer require --dev orm-fixtures
```

## Création des fixtures

Pour créer des fixtures, il faut créer une classe qui hérite de `Fixture` et qui implémente la méthode `load` :

```php
<?php

namespace App\DataFixtures;

use Doctrine\Bundle\FixturesBundle\Fixture;
use Doctrine\Persistence\ObjectManager;

class AppFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        for ($i = 0; $i < 10; $i++) {
            $product = new Product();
            $product->setName('Product ' . $i);
            $product->setPrice(rand(10, 100));
            $manager->persist($product);
        }

        $manager->flush();
    }
}
```

## Chargement des fixtures

Pour charger les fixtures, il faut exécuter la commande suivante :

```bash
php bin/console doctrine:fixtures:load
```

Pour ajouter des fixtures sans supprimer les données existantes, il faut ajouter l'option `--append` :

```bash
php bin/console doctrine:fixtures:load --append
```

Pour éviter la confirmation de la suppression des données existantes, il faut ajouter l'option `-n` :

```bash
php bin/console doctrine:fixtures:load -n
```

## Faker

Faker est une bibliothèque PHP qui permet de générer des données aléatoires. Pour l'utiliser, il faut ajouter le package `fzaninotto/faker` à votre projet :

```bash
composer require fzaninotto/faker
```

Pour générer des données aléatoires, il faut créer une instance de `Faker` :

```php
use Faker\Factory;

$faker = Factory::create();

echo $faker->name;
```

Pour plus d'informations, consultez la [documentation de Faker](https://fakerphp.org/).

## Conclusion

Les fixtures sont un outil essentiel pour peupler la base de données avec des données de test. Elles permettent de tester l'application dans des conditions réelles sans affecter les données existantes.

---

[🏠 Retour au sommaire](#)
