# Cours Complet sur HTML, CSS et HTML Sémantique

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

