### Table des matières du syllabus :
1. **Introduction à Symfony 7**
2. **Installation de Symfony**
3. **Structure d'un projet Symfony**
4. **Les routes et les contrôleurs**
5. **Les vues et Twig**
6. **Les formulaires Symfony**
7. **Gestion des bases de données avec Doctrine**
8. **Sécurisation de l'application**
9. **Tests avec Symfony**
10. **API REST avec Symfony**
11. **Création d'un bundle personnalisé**
12. **Déploiement et optimisation**

---

### 1. Introduction à Symfony 7

Symfony 7 est la dernière version majeure de Symfony. C’est un framework PHP qui permet de créer des applications web complexes de manière structurée, maintenable et flexible. Symfony est basé sur des composants réutilisables qui peuvent être utilisés dans n'importe quelle application PHP, ce qui permet de construire des projets modulaires et extensibles.

### 2. Installation de Symfony

Pour installer Symfony, vous devez avoir PHP et Composer installés. Voici comment installer Symfony :

#### a. Installation via Symfony CLI

```bash
curl -sS https://get.symfony.com/cli/installer | bash
```

Cela vous permet d'installer la CLI Symfony qui facilite la gestion des projets Symfony.

#### b. Créer un projet Symfony

```bash
symfony new my_project_name --full
```

Cela crée un projet Symfony complet avec toutes les fonctionnalités par défaut (routeur, sécurité, etc.).

#### c. Installation via Composer

```bash
composer create-project symfony/skeleton my_project_name
```

Cela installe un projet Symfony minimaliste. Vous devrez ensuite installer les composants nécessaires selon vos besoins.

### 3. Structure d'un projet Symfony

Un projet Symfony a une structure de répertoires standard :

```
my_project_name/
├── bin/                # Fichiers exécutables (par exemple : console)
├── config/             # Configuration de l'application
├── public/             # Fichiers accessibles publiquement (ex. : index.php)
├── src/                # Code source de l'application (contrôleurs, services, etc.)
├── templates/          # Vues Twig
├── translations/       # Fichiers de traduction
├── var/                # Fichiers générés (cache, logs)
├── vendor/             # Dépendances installées via Composer
```

### 4. Les routes et les contrôleurs

#### a. Définir une route

Les routes dans Symfony sont définies dans les contrôleurs ou via des annotations. Voici un exemple de route dans un contrôleur avec une annotation.

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class DefaultController
{
    /**
     * @Route("/", name="home")
     */
    public function index()
    {
        return new Response('Hello Symfony!');
    }
}
```

#### b. Utiliser les routes avec des paramètres

Les routes peuvent inclure des paramètres dans l'URL.

```php
// src/Controller/ArticleController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class ArticleController
{
    /**
     * @Route("/article/{slug}", name="article_show")
     */
    public function show($slug)
    {
        return new Response("Article : " . $slug);
    }
}
```

### 5. Les vues et Twig

**Twig** est le moteur de templates par défaut dans Symfony. Il vous permet de séparer la logique de présentation de votre logique d'application.

#### a. Exemple d'une vue avec Twig

```php
// src/Controller/DefaultController.php
namespace App\Controller;

use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class DefaultController extends AbstractController
{
    /**
     * @Route("/", name="home")
     */
    public function index()
    {
        return $this->render('home/index.html.twig', [
            'message' => 'Hello Symfony with Twig!'
        ]);
    }
}
```

#### b. Exemple d'un fichier Twig (`templates/home/index.html.twig`)

```twig
<!DOCTYPE html>
<html>
<head>
    <title>Symfony 7 - Home</title>
</head>
<body>
    <h1>{{ message }}</h1>
</body>
</html>
```

### 6. Les formulaires Symfony

Symfony fournit un composant Formulaire très puissant. Voici un exemple de formulaire simple.

#### a. Créer un formulaire

```php
// src/Form/Type/ContactType.php
namespace App\Form;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\FormInterface;

class ContactType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options)
    {
        $builder
            ->add('name', TextType::class)
            ->add('email', TextType::class)
            ->add('send', SubmitType::class);
    }
}
```

#### b. Traitement du formulaire dans un contrôleur

```php
// src/Controller/ContactController.php
namespace App\Controller;

use App\Form\ContactType;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

class ContactController extends AbstractController
{
    /**
     * @Route("/contact", name="contact")
     */
    public function index(Request $request)
    {
        $form = $this->createForm(ContactType::class);

        $form->handleRequest($request);

        if ($form->isSubmitted() && $form->isValid()) {
            // Traitez les données du formulaire ici
            return new Response('Formulaire envoyé !');
        }

        return $this->render('contact/index.html.twig', [
            'form' => $form->createView(),
        ]);
    }
}
```

#### c. Vue Twig pour le formulaire

```twig
{# templates/contact/index.html.twig #}
{{ form_start(form) }}
{{ form_row(form.name) }}
{{ form_row(form.email) }}
{{ form_row(form.send) }}
{{ form_end(form) }}
```

### 7. Gestion des bases de données avec Doctrine

Symfony utilise Doctrine pour interagir avec les bases de données. Voici comment vous pouvez définir des entités et interagir avec la base de données.

#### a. Créer une entité

```php
// src/Entity/Article.php
namespace App\Entity;

use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Entity
 * @ORM\Table(name="article")
 */
class Article
{
    /**
     * @ORM\Id
     * @ORM\GeneratedValue
     * @ORM\Column(type="integer")
     */
    private $id;

    /**
     * @ORM\Column(type="string")
     */
    private $title;

    // Getters et Setters
}
```

#### b. Créer la base de données et les tables

```bash
php bin/console doctrine:database:create
php bin/console doctrine:schema:update --force
```

### 8. Sécurisation de l'application

Symfony offre un composant de sécurité robuste, permettant de gérer l'authentification, les rôles et les autorisations.

#### a. Configuration de la sécurité

```yaml
# config/packages/security.yaml
security:
    encoders:
        App\Entity\User:
            algorithm: bcrypt
    providers:
        in_memory:
            memory:
                users:
                    user:
                        password: "password"
                        roles: 'ROLE_USER'
    firewalls:
        main:
            pattern: ^/
            form_login:
                login_path: login
                check_path: login
            logout:
                path: logout
            anonymous: true
    access_control:
        - { path: ^/admin, roles: ROLE_ADMIN }
```

### 9. Tests avec Symfony

Symfony offre un excellent support pour les tests via PHPUnit.

#### a. Exemple de test fonctionnel

```php
// tests/Controller/DefaultControllerTest.php
namespace App\Tests\Controller;

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class DefaultControllerTest extends WebTestCase
{
    public function testIndex()
    {
        $client = static::createClient();
        $crawler = $client->request('GET', '/');

        $this->assertResponseIsSuccessful();
        $this->assertSelectorTextContains('h1', 'Hello Symfony!');
    }
}
```

### 10. API REST avec Symfony

Symfony permet de créer des API RESTful facilement, notamment avec des outils comme **API Platform**.

#### a. Exemple de ressource API

```php
// src/Entity/Book.php
namespace App\Entity;

use ApiPlatform\Core\Annotation\ApiResource;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ApiResource
 * @ORM\Entity
 */
class Book
{
    // Définissez vos propriétés ici
}
```

### 11. Création d'un bundle personnalisé

Les **bundles** sont des modules réutilisables dans Symfony. Vous pouvez créer un bundle pour organiser votre code.

#### a. Créer un bundle

```bash
php bin/console make:bundle
```

### 12. Déploiement et optimisation

Pour déployer Symfony en production, vous pouvez utiliser **Docker**, **Ansible**, ou tout autre outil d'automatisation. Il est important d'optimiser les performances avec des caches et des optimisations telles que **OPcache**.

#### a. Commande d'optimisation

```bash
php bin/console cache:clear --env=prod --no-debug
php bin/console cache:warmup --env=prod
```
