### **Chapitre 12 : Framework CSS Bootstrap**

#### **1. Introduction à Bootstrap et ses avantages**

##### **Qu'est-ce que Bootstrap ?**
Bootstrap est un framework CSS open-source qui permet de créer des sites web responsives, modernes et interactifs de manière rapide. Il offre un ensemble de styles CSS et de composants JavaScript prédéfinis pour simplifier le développement d'interfaces utilisateurs.

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
    <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="row">
            <div class="col-4">Colonne 1</div>
            <div class="col-4">Colonne 2</div>
            <div class="col-4">Colonne 3</div>
        </div>
    </div>
</body>
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

##### **Navbar (Barre de navigation)**
La **navbar** est un composant qui permet de créer une barre de navigation.

**Exemple d'une navbar simple :**

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="#">Mon Site</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
            <li class="nav-item active">
                <a class="nav-link" href="#">Accueil <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Fonctionnalités</a>
            </li>
            <li class="nav-item">
                <a class="nav-link" href="#">Prix</a>
            </li>
        </ul>
    </div>
</nav>
```

**Explication :**
- **`navbar-expand-lg` :** Cette classe permet de rendre la barre de navigation responsive. Elle se rétracte sur les écrans petits.
- **`navbar-light` :** Définit un thème clair pour la barre de navigation.
- **`navbar-toggler` :** Crée un bouton pour afficher le menu sur les petits écrans.

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
<!-- Button to trigger modal -->
<button type="button" class="btn btn-primary" data-toggle="modal" data-target="#myModal">
    Ouvrir le Modal
</button>

<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="myModalLabel">Titre du Modal</h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                Ceci est le contenu de la fenêtre modale.
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">Fermer</button>
                <button type="button" class="btn btn-primary">Sauvegarder les modifications</button>
            </div>
        </div>
    </div>
</div>
```

**Explication :**
- **`modal` :** Crée la fenêtre modale.
- **`data-toggle="modal"` et `data-target="#myModal"` :** Lient le bouton à la modale.
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