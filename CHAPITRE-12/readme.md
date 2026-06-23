### **Chapitre 12 : Framework CSS Bootstrap**

#### **1. Introduction à Bootstrap et ses avantages**

##### **Qu'est-ce que Bootstrap ?**
Bootstrap est un framework CSS open-source qui permet de créer des sites web responsives, modernes et interactifs de manière rapide. Il offre un ensemble de styles CSS et de composants JavaScript prédéfinis pour simplifier le développement d'interfaces utilisateurs.

**Documentation officielle :** https://getbootstrap.com/docs/5.3/getting-started/introduction/
##### **Avantages de Bootstrap :**
- **Mobile-first :** Par défaut, Bootstrap est conçu pour fonctionner sur les écrans mobiles et s'adapte automatiquement aux tailles d'écran plus grandes.
- **Gain de temps :** Il permet de créer des sites rapidement grâce à ses classes CSS et composants prêts à l'emploi.
- **Composants riches :** Il offre de nombreux composants comme des boutons, formulaires, modals, etc.
- **Support des navigateurs :** Fonctionne sur tous les navigateurs modernes.
- **Personnalisable :** Tu peux facilement personnaliser les couleurs, polices, et plus encore.

---

#### **2. Grille responsive et système de layout**

##### **Le système de grille de Bootstrap**
Bootstrap utilise un système de grille de 12 colonnes. Cela signifie que pour chaque ligne, tu peux diviser l’espace en jusqu'à 12 parts égales, ce qui te permet de créer des mises en page flexibles.

**Code de base d'une grille :**

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Grille Bootstrap</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <main class="container">
        <section class="row">
            <article class="col-4">Colonne 1</article>
            <article class="col-4">Colonne 2</article>
            <article class="col-4">Colonne 3</article>
        </section>
    </main>
</body>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</html>
```


**Explication :**
- **`container` :** Conteneur principal qui centre le contenu.
- **`row` :** Ligne qui contient des colonnes.
- **`col-4` :** Chaque colonne prend 4 sur 12 parts de la ligne (donc 3 colonnes au total).

##### **Mise en page responsive**
Bootstrap te permet de créer des mises en page qui s'ajustent en fonction de la taille de l'écran (responsive). On utilise les classes **`col-sm-*`, `col-md-*`, `col-lg-*`** pour gérer les tailles sur différentes résolutions.

**Exemple avec une grille responsive :**

```html
<div class="container">
    <div class="row">
        <div class="col-sm-12 col-md-4">Colonne 1</div>
        <div class="col-sm-12 col-md-4">Colonne 2</div>
        <div class="col-sm-12 col-md-4">Colonne 3</div>
    </div>
</div>
```

**Explication :**
- **`col-sm-12`** : Sur les petits écrans (mobile), chaque colonne occupe toute la largeur.
- **`col-md-4`** : Sur les écrans moyens (tablettes, écrans plus grands), chaque colonne prend 4 parts sur 12, donc 3 colonnes s'affichent côte à côte.

##### **Utilisation de Flexbox pour aligner les éléments**

Bootstrap inclut également Flexbox, une technologie CSS qui permet un alignement plus souple des éléments dans la grille.

**Exemple de flexbox dans une grille :**

```html
<div class="container">
    <div class="row d-flex justify-content-center">
        <div class="col-4">Colonne 1</div>
        <div class="col-4">Colonne 2</div>
        <div class="col-4">Colonne 3</div>
    </div>
</div>
```

**Explication :**
- **`d-flex` :** Applique Flexbox à la ligne.
- **`justify-content-center` :** Centre les éléments horizontalement.

---

#### **3. Composants prédéfinis (navbar, cards, modals, etc.)**


---

# 📌 Créer une Barre de Navigation Responsive avec Bootstrap 5

Une **barre de navigation** est un élément essentiel de toute interface web. Dans ce guide, nous allons créer une navbar responsive avec **Bootstrap 5**, qui s’adapte automatiquement aux différentes tailles d’écran.

---

## 🔹 Étape 1 : Inclure Bootstrap 5

Avant de commencer, assure-toi d'inclure **Bootstrap 5** dans ton projet. Ajoute les liens suivants dans la section `<head>` de ton fichier HTML :

```html
<!-- Bootstrap CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Bootstrap JS (nécessaire pour le menu burger) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
```

---

## 🔹 Étape 2 : Créer la structure de la barre de navigation `<nav>`

Voici la structure de base d’une **navbar Bootstrap** :

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
</nav>
```

### 🔍 Explication :
- **`navbar`** → Classe de base de Bootstrap pour une barre de navigation.
- **`navbar-expand-lg`** → La barre sera repliée (menu burger) sur les petits écrans et affichée en mode étendu à partir de la taille *large* (`lg`).
- **`navbar-light bg-light`** → Définit un fond clair et un texte sombre pour un bon contraste visuel.

---

## 🔹 Étape 3 : Ajouter le logo ou le titre du site

À l’intérieur de `<nav>`, on ajoute le **nom du site ou un logo** avec la classe `navbar-brand` :

```html
<a class="navbar-brand" href="#">Mon Site</a>
```

### 🔍 Explication :
- **`navbar-brand`** → Utilisé pour afficher un logo ou le nom du site.
- Il peut contenir un texte ou une **image (`<img>`)** si on veut un logo.

Exemple avec un logo :
```html
<a class="navbar-brand" href="#">
    <img src="logo.png" alt="Logo" width="40" height="40">
</a>
```

---

## 🔹 Étape 4 : Ajouter le bouton pour le menu mobile

Sur mobile, le menu se replie et devient un **menu burger**. On ajoute donc un bouton pour l’ouvrir :

```html
<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
    <span class="navbar-toggler-icon"></span>
</button>
```

### 🔍 Explication :
- **`navbar-toggler`** → Identifie ce bouton comme celui du menu.
- **`data-bs-toggle="collapse"`** → Active/désactive l’affichage du menu.
- **`data-bs-target="#navbarNav"`** → Associe le bouton au menu avec l’ID `navbarNav`.
- **`navbar-toggler-icon`** → Affiche l’icône du menu burger.

---

## 🔹 Étape 5 : Ajouter les liens du menu

On ajoute les liens de navigation dans un `<div>` contenant une liste `<ul>`.

```html
<div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
        <li class="nav-item active">
            <a class="nav-link" href="#">Accueil <span class="visually-hidden">(current)</span></a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="#">Fonctionnalités</a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="#">Tarifs</a>
        </li>
    </ul>
</div>
```

### 🔍 Explication :
- **`collapse navbar-collapse`** → Permet au menu de se replier en version mobile.
- **`navbar-nav`** → Spécifie une liste de liens de navigation.
- **`nav-item`** → Définit chaque élément du menu.
- **`nav-link`** → Applique un style standard aux liens.
- **`active`** → Indique la page actuelle (ici "Accueil").
- **`visually-hidden`** → Améliore l’accessibilité pour les lecteurs d’écran.

---

## 🔹 Étape 6 : Tester la navigation

En copiant ce code dans un fichier **HTML** et en l’ouvrant dans un navigateur, tu devrais voir une barre de navigation **responsive**, s’adaptant automatiquement aux écrans **mobiles et desktop**.

---

## ✅ Code Final Complet

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Menu Bootstrap</title>
    
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

    <!-- Bootstrap JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</head>
<body>

<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Mon Site</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
            <li class="nav-item active">
                <a class="nav-link" href="#">Accueil <span class="visually-hidden">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Fonctionnalités</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Tarifs</a>
            </li>
        </ul>
    </div>
</nav>

</body>
</html>
```

---

## 🚀 Résumé des étapes :

1️⃣ **Inclure Bootstrap (CSS + JS)**  
2️⃣ **Créer une `<nav>` avec les classes Bootstrap**  
3️⃣ **Ajouter le logo ou le nom du site (`navbar-brand`)**  
4️⃣ **Ajouter un bouton pour le menu mobile (`navbar-toggler`)**  
5️⃣ **Créer les liens de navigation dans une liste `<ul>`**  
6️⃣ **Tester l'affichage sur différentes tailles d'écran**

---
##### **Cards**
Les **cards** sont des éléments qui contiennent du contenu dans un format de "boîte".

**Exemple d’une card :**

```html
<div class="card" style="width: 18rem;">
    <img src="https://via.placeholder.com/150" class="card-img-top" alt="Image">
    <div class="card-body">
        <h5 class="card-title">Titre de la carte</h5>
        <p class="card-text">Ceci est une description dans la carte. Très utile pour du contenu complémentaire.</p>
        <a href="#" class="btn btn-primary">Aller quelque part</a>
    </div>
</div>
```

**Explication :**
- **`card` :** Crée une card.
- **`card-img-top` :** Image en haut de la card.
- **`card-body` :** Corps de la card qui contient le texte, le titre et le bouton.

##### **Modals**
Les **modals** sont des fenêtres modales utilisées pour afficher du contenu de manière temporaire.

**Exemple d’un modal :**

```html
<!-- Button to trigger modal (Bootstrap 5) -->
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#myModal">
    Ouvrir le Modal
</button>

<!-- Modal (Bootstrap 5) -->
<div class="modal fade" id="myModal" tabindex="-1" aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="myModalLabel">Titre du Modal</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                Ceci est le contenu de la fenêtre modale.
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Fermer</button>
                <button type="button" class="btn btn-primary">Sauvegarder les modifications</button>
            </div>
        </div>
    </div>
</div>
```

**Explication :**
- **`modal` :** Crée la fenêtre modale.
- **`data-bs-toggle="modal"` et `data-bs-target="#myModal"` :** Lient le bouton à la modale (Bootstrap 5 utilise le préfixe `bs-`).
- **`fade` :** Ajoute un effet de transition au modal.

---

#### **4. Personnalisation et intégration dans un projet web**

##### **Personnalisation de Bootstrap avec SASS**
Bootstrap permet de personnaliser facilement ses variables via SASS.

**Exemple de personnalisation d'une couleur de fond :**

1. **Installer Bootstrap via npm ou Yarn :**

```bash
npm install bootstrap
```

2. **Modifier les variables SASS :**
   Crée un fichier `custom.scss` où tu vas personnaliser les variables de Bootstrap.

```scss
// Importer les fichiers Bootstrap SASS
@import "~bootstrap/scss/bootstrap";

// Personnaliser la couleur de fond
$body-bg: #f0f0f0;
```

3. **Compiler le fichier SASS en CSS :**

```bash
sass custom.scss custom.css
```

4. **Inclure le fichier CSS personnalisé dans ton projet :**

```html
<link href="custom.css" rel="stylesheet">
```

**Explication :**
- Bootstrap utilise SASS pour permettre une personnalisation plus facile.
- En modifiant les variables comme `$body-bg`, tu changes l'apparence globale de ton site.

---