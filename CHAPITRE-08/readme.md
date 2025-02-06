## 1. Introduction à HTML (HyperText Markup Language)
HTML est le langage standard pour la création de pages web. Il permet de structurer le contenu à l'aide d'éléments (ou balises). Chaque élément définit une partie du document comme un paragraphe, une image, un lien ou une liste.

### 1.1 Structure de Base d'une Page HTML
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ma Page Web</title>
</head>
<body>
    <h1 id="titre-principal" class="titre">Bienvenue sur mon site</h1>
    <p class="paragraphe">Ceci est un paragraphe de texte.<br>Voici une nouvelle ligne.</p>
</body>
</html>
```

### 1.2 Éléments de Base
- `<h1>` à `<h6>` : Titres (du plus grand au plus petit)
- `<p>` : Paragraphe
- `<a href="">` : Lien hypertexte
- `<img src="" alt="">` : Image
- `<ul>`, `<ol>`, `<li>` : Listes (non ordonnées, ordonnées)
- `<div>` : Division de contenu (non sémantique)
- `<br>` : Saut de ligne
- `<b>` : Texte en gras (bold text)
- `<strong>` : Texte important (important text)
- `<i>` : Texte en italique (italic text)
- `<em>` : Texte mis en valeur (emphasized text)
- `<mark>` : Texte marqué (marked text)
- `<small>` : Texte plus petit (smaller text)
- `<del>` : Texte supprimé (deleted text)
- `<ins>` : Texte inséré (inserted text)
- `<sub>` : Texte en indice (subscript text)
- `<sup>` : Texte en exposant (superscript text)
- `<blockquote>` : Citation longue (block quotation)
- `<q>` : Citation courte (short quotation)
- `<abbr>` : Abréviation (abbreviation)
- `<address>` : Informations de contact (address)
- `<cite>` : Titre d'œuvre (citation)
- `<bdo>` : Inversion de direction du texte (Bi-Directional Override)
- `<root>` : L'élément racine personnalisé (HTML5+)
- **`id` et `class`** : Attributs utilisés pour identifier et styliser les éléments HTML.

**Différence entre `id` et `class` :**
- **`id`** : Identifiant unique dans une page. Utilisé pour sélectionner un seul élément spécifique.
- **`class`** : Attribut non unique pouvant être attribué à plusieurs éléments. Permet de styliser un groupe d'éléments.

Exemple :
```html
<h1 id="titre-unique">Titre unique</h1>
<p class="texte-paragraphe">Paragraphe avec une classe.</p>
```

---

## 2. Introduction à CSS (Cascading Style Sheets)
CSS est utilisé pour styliser les éléments HTML. Il permet de séparer la présentation du contenu.

### 2.1 Syntaxe de Base de CSS
```css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
}
.titre {
    color: #333;
    text-align: center;
}
.texte-paragraphe {
    font-size: 16px;
}
```

### 2.2 Intégration du CSS
Trois méthodes principales :
- **Interne** : Dans une balise `<style>` dans le `<head>`.
- **Externe** : Dans un fichier `.css` séparé (meilleure pratique).
- **En ligne** : Directement dans une balise HTML via l'attribut `style`.

```html
<head>
    <link rel="stylesheet" href="style.css">
</head>
```

---

## 3. HTML Sémantique
HTML sémantique signifie utiliser des balises qui ont une signification claire, améliorant l'accessibilité et le référencement.

### 3.1 Pourquoi Utiliser le HTML Sémantique ?
- **Accessibilité** : Aide les lecteurs d'écran et autres technologies d'assistance.
- **SEO (Search Engine Optimization)** : Améliore le référencement des pages.
- **Maintenance** : Rend le code plus clair et plus facile à maintenir.

### 3.2 Balises Sémantiques Courantes
- `<header>` : En-tête de la page ou d'une section
- `<nav>` : Barre de navigation
- `<main>` : Contenu principal
- `<section>` : Section d'un document
- `<article>` : Article indépendant
- `<aside>` : Contenu secondaire (barre latérale)
- `<footer>` : Pied de page
- `<br>` : Saut de ligne
- `<root>` : Définit un conteneur racine principal pour regrouper tout le contenu HTML

---
# Cours complet sur CSS : Marges, Espacements, Dimensions et Flexbox

## 1. Marges, Espacements et Bordures

### 1.1 `margin`
La propriété `margin` définit l'espace extérieur autour d'un élément.

```css
.element {
    margin: 10px; /* Marge de 10px sur tous les côtés */
    margin: 10px 20px; /* 10px en haut et bas, 20px à gauche et à droite */
    margin: 10px 15px 20px 25px; /* Haut, Droite, Bas, Gauche */
}
```

### 1.2 `padding`
La propriété `padding` définit l'espace intérieur d'un élément, entre son contenu et sa bordure.

```css
.element {
    padding: 10px; /* Padding uniforme */
    padding: 10px 20px; /* 10px en haut/bas, 20px gauche/droite */
}
```

### 1.3 `border`
La propriété `border` définit une bordure autour d'un élément.

```css
.element {
    border: 2px solid black;
    border-radius: 5px; /* Coins arrondis */
}
```

## 2. Dimensions et Redimensionnement

### 2.1 Largeur et Hauteur
Les propriétés `width` et `height` définissent la taille d’un élément.

```css
.element {
    width: 100px;
    height: 50px;
    max-width: 500px;
    min-width: 100px;
}
```

### 2.2 Box-sizing
Définit la manière dont la taille totale est calculée.

```css
.element {
    box-sizing: border-box; /* Inclut padding et border dans la taille totale */
}
```

## 3. Unités de Mesure en CSS

### 3.1 Unités Absolues
- `px` (pixels) : unité fixe.
- `cm`, `mm`, `in`, `pt`, `pc` : rarement utilisées en web.

### 3.2 Unités Relatives
- `%` : proportionnelle à l’élément parent.
- `em` : proportionnelle à la taille de la police du parent.
- `rem` : proportionnelle à la taille de la police racine.
- `vw` / `vh` : pourcentage de la largeur / hauteur de la fenêtre.
- `vmin` / `vmax` : plus petit ou plus grand des deux (`vw` ou `vh`).

## 4. Flexbox

### 4.1 Introduction
Flexbox est un modèle de disposition en CSS conçu pour organiser et aligner les éléments d'une page de manière efficace, notamment lorsqu'il s'agit de mise en page responsive.

### 4.2 Conteneur Flex
Un élément devient un conteneur Flex lorsque l'on applique `display: flex` ou `display: inline-flex`.

```css
.container {
    display: flex; /* Ou inline-flex */
}
```

### 4.3 Les éléments flexibles (Flex Items)
Les enfants directs d'un conteneur flex deviennent des éléments flexibles.

### 4.4 Propriétés du conteneur Flex

#### `flex-direction`
Définit la direction principale des éléments.

```css
.container {
    flex-direction: row; /* Par défaut */
    flex-direction: column;
}
```

#### `justify-content`
Gère l'alignement des éléments sur l'axe principal.

```css
.container {
    justify-content: center;
    justify-content: space-between;
}
```

#### `align-items`
Gère l'alignement des éléments sur l'axe secondaire.

```css
.container {
    align-items: center;
    align-items: flex-end;
}
```

#### `flex-wrap`
Permet de gérer le retour à la ligne des éléments si nécessaire.

```css
.container {
    flex-wrap: wrap;
}
```

### 4.5 Propriétés des éléments flexibles

#### `flex`
Shorthand pour `flex-grow`, `flex-shrink` et `flex-basis`.

```css
.item {
    flex: 1 1 200px;
}
```

#### `align-self`
Permet d'aligner un élément différemment des autres.

```css
.item {
    align-self: center;
}
```

---

## 5. HTML Forms
Les formulaires HTML permettent de collecter des données de l'utilisateur.

### 5.1 Exemple de Formulaire Simple
```html
<form action="/action_page.php">
  <label for="fname">Prénom :</label><br>
  <input type="text" id="fname" name="fname" value="Jean"><br>
  <label for="lname">Nom :</label><br>
  <input type="text" id="lname" name="lname" value="Dupont"><br><br>
  <input type="submit" value="Envoyer">
</form>
```

### 5.2 Attributs des Champs de Formulaire
- **value** : Définit une valeur initiale pour le champ.
- **readonly** : Rend le champ en lecture seule.
- **disabled** : Désactive le champ (non éditable et non envoyable).
- **size** : Définit la largeur visible du champ (en caractères).
- **maxlength** : Nombre maximum de caractères autorisés.
- **min et max** : Définit des valeurs minimales et maximales pour les champs numériques et de date.
- **multiple** : Permet la sélection de plusieurs valeurs.
- **pattern** : Expression régulière pour la validation du champ.
- **placeholder** : Texte indicatif dans le champ.
- **required** : Rend le champ obligatoire.
- **step** : Définit des intervalles de valeur.
- **autofocus** : Donne le focus automatique au chargement de la page.

Exemple :
```html
<form>
  <label for="username">Nom d'utilisateur :</label>
  <input type="text" id="username" name="username" required autofocus>
</form>
```
# &#x20;Responsive Design et les Media Queries

## Introduction

Le **Responsive Design** est une approche du design web qui permet à une page web de s'adapter automatiquement à la taille de l'écran de l'utilisateur. Il garantit une expérience utilisateur optimale sur tous les appareils (ordinateurs, tablettes, smartphones, etc.).

Les **Media Queries** sont une fonctionnalité de CSS qui permet d'appliquer des styles différents en fonction de la taille de l'écran, de l'orientation ou d'autres caractéristiques du dispositif.

---

## 1. Les Fondements du Responsive Design

### 1.1. Les Principes du Responsive Design

Le responsive design repose sur trois principes fondamentaux :

1. **Les grilles flexibles** : Utilisation de **grilles fluides** pour adapter les mises en page aux différents écrans.
2. **Les images flexibles** : Les images doivent être redimensionnées dynamiquement pour ne pas déborder.
3. **Les Media Queries** : Utilisation de CSS pour adapter l'affichage en fonction de la taille de l'écran.

### 1.2. Exemple de Grille Flexible avec Flexbox

```css
.container {
    display: flex;
    flex-wrap: wrap;
}

.item {
    flex: 1 1 300px; /* Minimum 300px, s'adapte au reste */
    margin: 10px;
    padding: 20px;
    background-color: lightblue;
    text-align: center;
}
```

```html
<div class="container">
    <div class="item">Bloc 1</div>
    <div class="item">Bloc 2</div>
    <div class="item">Bloc 3</div>
</div>
```

---

## 2. Les Media Queries

### 2.1. Syntaxe de Base

```css
@media (condition) {
    /* Règles CSS à appliquer */
}
```

### 2.2. Exemple de Media Query

```css
body {
    background-color: white;
}

@media (max-width: 768px) {
    body {
        background-color: lightgray;
    }
}
```

Dans cet exemple, le fond de la page passe de blanc à gris clair lorsque la largeur de l'écran est inférieure à **768px**.

### 2.3. Media Queries pour Différents Appareils

```css
/* Pour les écrans larges (ordinateurs) */
@media (min-width: 1024px) {
    body {
        font-size: 18px;
    }
}

/* Pour les tablettes */
@media (max-width: 1023px) and (min-width: 768px) {
    body {
        font-size: 16px;
    }
}

/* Pour les smartphones */
@media (max-width: 767px) {
    body {
        font-size: 14px;
    }
}
```

---

## 3. Techniques Avancées de Responsive Design

### 3.1. Utilisation de CSS Grid pour un Layout Flexible

```css
.container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 20px;
}

.item {
    background-color: lightcoral;
    padding: 20px;
    text-align: center;
}
```

### 3.2. Images et Vidéos Responsives

```css
img {
    max-width: 100%;
    height: auto;
}

video {
    max-width: 100%;
    height: auto;
}
```

### 3.3. Menu de Navigation Responsive

```css
.navbar {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 10px;
    background-color: black;
}

.navbar a {
    color: white;
    text-decoration: none;
    padding: 10px;
}

@media (max-width: 768px) {
    .navbar {
        flex-direction: column;
    }
    .navbar a {
        text-align: center;
        padding: 15px;
    }
}
```

# **Pseudo elements `::after` et `::before` en CSS**  

Les pseudo-éléments `::after` et `::before` permettent d'ajouter du contenu **avant ou après** un élément HTML **sans modifier le code HTML**. Ils sont souvent utilisés pour ajouter des icônes, des effets décoratifs, ou du contenu dynamique.

---

## **1. Différence entre `::before` et `::after`**
| Pseudo-élément | Explication |
|---------------|------------|
| `::before` | Ajoute du contenu **avant** l’élément ciblé. |
| `::after` | Ajoute du contenu **après** l’élément ciblé. |

📌 **Règle importante :**  
Les pseudo-éléments `::before` et `::after` nécessitent **obligatoirement** la propriété `content` pour fonctionner.

---

## **2. Syntaxe Générale**
```css
élément::before {
    content: "Texte avant";
}

élément::after {
    content: "Texte après";
}
```
📌 **Explication :**
- `élément` : L’élément HTML ciblé (ex : `p`, `a`, `div`, etc.).
- `content` : Définit ce qui est ajouté avant ou après l’élément.

---

## **3. Exemples Pratiques**
### **3.1 Ajouter une décoration avant et après un élément**
```html
<p>Bienvenue sur mon site</p>
```
```css
p::before {
    content: "🔥 ";
}

p::after {
    content: " 🎉";
}
```
📌 **Résultat :**  
🔥 Bienvenue sur mon site 🎉

---

### **3.2 Ajouter une icône après un lien (`a::after`)**
```html
<a href="https://www.google.com" target="_blank">Google</a>
```
```css
a[target="_blank"]::after {
    content: " 🔗";
    font-size: 12px;
    color: gray;
}
```
📌 **Résultat :**  
Le lien **Google** aura une icône `🔗` indiquant qu’il s’ouvre dans un nouvel onglet.

---

### **3.3 Ajouter un effet de soulignement dynamique sous un lien**
```css
a::after {
    content: "";
    display: block;
    width: 0%;
    height: 2px;
    background-color: blue;
    transition: width 0.3s ease-in-out;
}

a:hover::after {
    width: 100%;
}
```
📌 **Explication :**
- Un **trait bleu sous le lien** apparaît uniquement **au survol** (`hover`).
- Il commence à **0%** et s’étend sur **100%** avec une **animation fluide**.

---

### **3.4 Ajouter des guillemets autour d’un texte**
```css
blockquote::before {
    content: "« ";
    font-size: 20px;
    color: gray;
}

blockquote::after {
    content: " »";
    font-size: 20px;
    color: gray;
}
```
📌 **Résultat :**  
**« Mon texte dans une citation »**  
Les guillemets sont ajoutés automatiquement.

---

### **3.5 Afficher l’URL d’un lien après `<a>`**
```css
a::after {
    content: " (" attr(href) ")";
    font-size: 12px;
    color: gray;
}
```
📌 **Explication :**  
- `attr(href)` affiche l’attribut `href` de `<a>`.  
- Très utile pour indiquer une URL sur des documents imprimés.

---

## **4. `::before` et `::after` avec des images (via `background`)**
Les pseudo-éléments **ne peuvent pas contenir d’images directement** via `content`. Mais on peut utiliser `background-image`.

```css
button::before {
    content: "";
    display: inline-block;
    width: 20px;
    height: 20px;
    background-image: url('icone.png');
    background-size: cover;
    margin-right: 5px;
}
```
📌 **Explication :**  
- Un **icône est ajouté avant le bouton** en utilisant une image.

---

## **5. Utilisation Avancée : Création d’un Badge**
### **5.1 Ajout d’un badge "Nouveau" sur un produit**
```html
<div class="produit">Produit X</div>
```
```css
.produit {
    position: relative;
    display: inline-block;
    padding: 10px;
}

.produit::after {
    content: "Nouveau";
    position: absolute;
    top: -10px;
    right: -10px;
    background-color: red;
    color: white;
    font-size: 12px;
    padding: 5px;
    border-radius: 5px;
}
```
📌 **Résultat :**  
Un **badge rouge "Nouveau"** est ajouté en haut à droite du produit.

---

## **6. Résumé et Bonnes Pratiques**
✅ **Utiliser `::before` et `::after` pour ajouter du contenu décoratif.**  
✅ **Toujours inclure `content: ""` même si aucun texte n’est ajouté.**  
✅ **Ne pas utiliser ces pseudo-éléments pour du contenu essentiel** (ils sont ignorés par certains lecteurs d'écran).  
✅ **Utiliser `position: absolute;` pour placer des éléments supplémentaires comme des badges.**  



## Documentation (L'un des meilleurs développeur frontend spécialisé en CSS):  
[text](https://www.youtube.com/watch?v=N5wpD9Ov_To)
