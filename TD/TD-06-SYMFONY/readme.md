## Symfony 6 CRUD

Dans cette procédure, nous allons vous expliquer comment créer un CRUD à l’aide du Framework PHP **Symfony 6**. La création d’un CRUD permet de gérer une multitude de cas d’utilisation dans vos applications.

### Prérequis

Avant de commencer, vous devez installer **Symfony 6** et **Composer**. Voici comment procéder :

---

### 1. Installation de Symfony et Composer

#### Installer Composer
**Composer** est un gestionnaire de dépendances pour PHP, essentiel pour utiliser Symfony. Si vous ne l'avez pas encore installé, voici les étapes pour l'installer sur une machine **Debian** :

1. Téléchargez et installez Composer :

```bash
curl -sS https://getcomposer.org/installer | php
```

2. Déplacez Composer dans un dossier accessible globalement (pour qu'il soit disponible dans tous les projets) :

```bash
sudo mv composer.phar /usr/local/bin/composer
```

Vérifiez que Composer est installé correctement :

```bash
composer --version
```

#### Installer Symfony
Vous devez ensuite installer **Symfony**. Si ce n'est pas déjà fait, vous pouvez installer Symfony via **Symfony Installer** ou directement via Composer.

Pour installer Symfony via **Symfony Installer**, exécutez les commandes suivantes :

1. Téléchargez le **Symfony Installer** :

```bash
curl -sS https://get.symfony.com/cli/installer | bash
```

2. Déplacez l'exécutable dans un répertoire accessible globalement :

```bash
sudo mv ~/symfony*/bin/symfony /usr/local/bin/symfony
```

Vérifiez que Symfony est installé correctement :

```bash
symfony -v
```

---

### 2. Création du Projet CRUD avec Symfony 6

#### Créer un nouveau projet Symfony

Une fois que Symfony et Composer sont installés, créez un nouveau projet Symfony en exécutant la commande suivante :

```bash
symfony new projet_crud --webapp
```

Cela créera une nouvelle application Symfony avec la structure de base pour une application web.

#### Se rendre dans le dossier du projet

Ensuite, naviguez dans le dossier de votre projet nouvellement créé :

```bash
cd projet_crud
```

#### Configurer la connexion à la base de données

Ouvrez le fichier `.env` et modifiez la variable `DATABASE_URL` pour spécifier votre base de données. Par exemple, pour une base MySQL locale :

```bash
DATABASE_URL="mysql://symfony:password@127.0.0.1:3306/projet_crud?serverVersion=mariadb-10.5.15&amp;charset=utf8mb4"
```

#### Créer la base de données

Symfony utilise **Doctrine** comme ORM (Object-Relational Mapper). Doctrine permet de mapper les entités PHP aux tables de la base de données. Pour créer la base de données, exécutez la commande suivante :

```bash
symfony console doctrine:database:create
```

Cela créera la base de données en fonction de la configuration de votre fichier `.env`.

---

### 3. Création de l'Entité Article

#### Créer l'entité

Pour créer l’entité `Article`, qui représentera les articles dans votre application, exécutez la commande suivante :

```bash
symfony console make:entity Article
```

Suivez les instructions pour ajouter les propriétés de l’entité :

- **id** (type : `int`)
- **date_publication** (type : `Date`)
- **titre** (type : `String`, longueur : 50)
- **contenu** (type : `Text`)

Doctrine générera alors les fichiers nécessaires pour votre entité et son repository.

#### Migration de la base de données

Une fois l’entité créée, générez la migration Doctrine avec la commande suivante :

```bash
symfony console make:migration
```

Puis appliquez la migration à la base de données avec :

```bash
symfony console doctrine:migrations:migrate
```

Cela va créer la table `article` dans la base de données.

---

### 4. Importation de Bootstrap dans Symfony 6

Pour améliorer l'apparence de l'interface utilisateur, nous allons importer **Bootstrap 5.3** via Webpack. Voici les étapes pour l'intégrer.

#### Installer Webpack Encore

Webpack Encore est l'outil de Symfony pour gérer les assets (CSS, JS, images). Pour l'installer, exécutez la commande suivante :

```bash
composer require symfony/webpack-encore-bundle
```

#### Installer les dépendances avec NPM

Installez les dépendances de Webpack avec la commande suivante :

```bash
npm install
```

#### Modifier le fichier `base.html.twig`

Dans le dossier `templates/`, ouvrez le fichier `base.html.twig` et ajoutez le lien vers **Bootstrap 5.3** :

```html
<!DOCTYPE html>
<html lang="fr">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>{% block title %}Welcome!{% endblock %}</title>
        <!-- CSS only -->
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet">
        {% block stylesheets %}{{ encore_entry_link_tags('app') }}{% endblock %}
    </head>
    <body>
        <div class="container mt-4">
            {% block body %}{% endblock %}
            {% block javascripts %}{{ encore_entry_script_tags('app') }}{% endblock %}
        </div>
    </body>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js"></script>
</html>
```

#### Vérification de la compilation des assets

Vérifiez que tout fonctionne bien en compilant les assets avec :

```bash
npm run build
```

---

### 5. Création du CRUD avec Symfony 6

#### Génération du CRUD

Maintenant, nous allons générer le **CRUD** pour l'entité `Article`. Exécutez la commande suivante :

```bash
symfony console make:crud
```

Répondez aux questions de la CLI pour configurer le CRUD :

- **Class name of the entity** : `Article`
- **Name of the controller class** : `ArticleController`
- **Generate tests for the controller?** : `no`

Symfony générera alors le contrôleur et les templates nécessaires.

#### Configuration des formulaires avec Bootstrap

Pour styliser les formulaires générés par Symfony, ajoutez la ligne suivante dans le fichier `config/packages/twig.yaml` :

```yaml
twig:
    form_themes: ['bootstrap_5_layout.html.twig']
```

Cela applique directement le thème Bootstrap à tous les formulaires.

---

### 6. Personnalisation des Templates

Voici quelques exemples de templates Twig personnalisés pour le projet.

#### Template `index.html.twig`

```twig
{% extends 'base.html.twig' %}

{% block title %}Article index{% endblock %}

{% block body %}
    <h1>Article index</h1>

    <table class="table">
        <thead>
            <tr>
                <th>Id</th>
                <th>Date_publication</th>
                <th>Titre</th>
                <th>Contenu</th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody>
        {% for article in articles %}
            <tr>
                <td>{{ article.id }}</td>
                <td>{{ article.datePublication ? article.datePublication|date('Y-m-d') : '' }}</td>
                <td>{{ article.titre }}</td>
                <td>{{ article.contenu }}</td>
                <td>
                    <a href="{{ path('app_article_show', {'id': article.id}) }}" class="btn btn-primary">Voir</a>
                    <a href="{{ path('app_article_edit', {'id': article.id}) }}" class="btn btn-success">Éditer</a>
                </td>
            </tr>
        {% else %}
            <tr>
                <td colspan="5">Aucun résultat</td>
            </tr>
        {% endfor %}
        </tbody>
    </table>

    <a href="{{ path('app_article_new') }}" class="btn btn-primary">Créer un nouvel article</a>
{% endblock %}
```

