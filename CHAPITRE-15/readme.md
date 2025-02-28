## Cours JavaScript DOM | BOM et jQuery avec AJAX et Requêtes Asynchrones

### Module 1: Introduction au DOM

#### 1.1 Qu'est-ce que le DOM ?
   - Le DOM (Document Object Model) représente la structure d'une page HTML sous forme d'arbre d'objets.
   - Il permet la manipulation dynamique de la structure, du style et du contenu d'une page.

     ![image](https://github.com/yugmerabtene/ESIEA-FISE-WEB-2024/assets/3670077/280d1040-3b8c-4dbe-95ca-c2377b9814d4)


#### 1.2 Accès au DOM avec JavaScript
   - Utilisation de `document` pour accéder au DOM.
   - Sélection d'éléments par ID, classe, balise.

   ```javascript
   // JavaScript
   let elementById = document.getElementById('monElementId');
   let elementsByClass = document.getElementsByClassName('maClasse');
   let elementsByTag = document.getElementsByTagName('div');
   ```

#### 1.3 Manipulation du contenu avec JavaScript
   - Modification du texte et de l'HTML d'un élément.
   - Ajout et suppression d'éléments.

   ```javascript
   // JavaScript
   elementById.textContent = 'Nouveau texte';
   elementById.innerHTML = '<strong>Nouveau texte en gras</strong>';

   let newElement = document.createElement('p');
   newElement.textContent = 'Contenu du nouvel élément';

   document.body.appendChild(newElement);
   document.body.removeChild(elementById);
   ```

#### 1.4 Accès au DOM avec jQuery
   - Sélection d'éléments par ID, classe, balise.

   ```javascript
   // jQuery
   let elementById = $('#monElementId');
   let elementsByClass = $('.maClasse');
   let elementsByTag = $('div');
   ```

#### 1.5 Manipulation du contenu avec jQuery
   - Modification du texte et du HTML d'un élément.
   - Ajout et suppression d'éléments.

   ```javascript
   // jQuery
   elementById.text('Nouveau texte');
   elementById.html('<strong>Nouveau texte en gras</strong>');

   let newElement = $('<p>').text('Contenu du nouvel élément');
   $('body').append(newElement);
   elementById.remove();
   ```

#### Exercices
   - Sélectionnez un élément par ID avec JavaScript et changez son texte, puis faites de même avec jQuery.
   - Créez un nouvel élément et ajoutez-le à la page avec JavaScript, puis faites de même avec jQuery.

### Module 2: Événements et gestionnaires d'événements

#### 2.1 Introduction aux événements
   - Types d'événements (clic, survol, etc.).
   - Attribution d'événements via JavaScript.

   ```javascript
   // JavaScript
   elementById.addEventListener('click', function() {
       alert('Élément cliqué !');
   });
   ```

#### 2.2 Gestionnaires d'événements
   - Utilisation de `addEventListener`.
   - Fonctions de gestion d'événements.

   ```javascript
   // JavaScript
   function handleClick() {
       alert('Élément cliqué !');
   }

   elementById.addEventListener('click', handleClick);
   ```

#### 2.3 Événements de formulaire
   - Validation de formulaire.
   - Soumission de formulaire.

   ```javascript
   // JavaScript
   document.getElementById('monFormulaire').addEventListener('submit', function(event) {
       event.preventDefault();
       // Effectuez vos actions de validation ici
   });
   ```

#### 2.4 Événements avec jQuery
   - Attribution d'événements avec jQuery.

   ```javascript
   // jQuery
   elementById.on('click', function() {
       alert('Élément cliqué !');
   });
   ```

#### Exercices
   - Créez un bouton qui affiche une alerte lorsqu'il est cliqué avec JavaScript, puis faites de même avec jQuery.
   - Validez un formulaire en utilisant JavaScript, puis faites de même avec jQuery.

### Module 3: Manipulation du style, des classes, fonctions et callbacks

#### 3.1 Modification du style
   - Changement de propriétés CSS.
   - Animation avec JavaScript.

   ```javascript
   // JavaScript
   elementById.style.color = 'red';

   // Animation avec classe CSS
   elementById.classList.add('animate');
   ```

#### 3.2 Manipulation des classes
   - Ajout et suppression de classes.
   - Toggle de classes.

   ```javascript
   // JavaScript
   elementById.classList.add('maNouvelleClasse');
   elementById.classList.remove('ancienneClasse');
   elementById.classList.toggle('toggleClasse');
   ```

#### 3.3 Fonctions en JavaScript
   - Définition de fonctions.
   - Passage de paramètres.
   - Retour de valeurs.

   ```javascript
   // JavaScript
   function additionner(a, b) {
       return a + b;
   }

   let resultat = additionner(3, 4);
   ```

#### 3.4 Callbacks en JavaScript
   - Utilisation de fonctions de rappel.
   - Gestion asynchrone avec des callbacks.

   ```javascript
   // JavaScript
   function effectuerCalcul(a, b, callback) {
       let resultat = a + b;
       callback(resultat);
   }

   function afficherResultat(resultat) {
       console.log('Le résultat est :', resultat);
   }

   effectuerCalcul(5, 7, afficherResultat);
   ```

#### Exercices
   - Créez une fonction en JavaScript qui prend deux paramètres et les multiplie, puis affichez le résultat.
   - Utilisez une fonction de rappel pour effectuer une opération asynchrone (simulée) en JavaScript.

### Module 4: Traverser et manipuler la structure

#### 4.1 Traverser les éléments
   - Parcours des éléments parents, enfants et frères.
   - Utilisation de `parentNode`, `childNodes`, `nextSibling`, etc.

   ```javascript
   // JavaScript
   let parentElement = elementById.parentNode;
   let childElements = elementById.childNodes;
   let nextSibling = elementById.nextSibling;
   ```

#### 4.2 Manipulation de l'arbre DOM
   - Clonage d'éléments.
   - Suppression d'éléments.

   ```javascript
   // JavaScript
   let clonedElement = elementById.cloneNode(true);
   document.body.removeChild(elementById);
   ```

#### 4.3 Traverser les éléments avec jQuery
   - Parcours des éléments parents, enfants et frères.

   ```javascript
   // jQuery
   let parentElement = elementById.parent();
   let childElements = elementById.children();
   let nextSibling = elementById.next();
   ```

#### 4.4 Manipulation de l'arbre DOM avec jQuery
   - Clonage d'éléments.
   - Suppression d'éléments.

```javascript
   // jQuery
   let clonedElement = elementById.clone();
   elementById.remove();
   ```

### Module 5: AJAX et Requêtes Asynchrones

#### 5.1 AJAX avec JavaScript
   - Utilisation de l'objet `XMLHttpRequest` pour effectuer des requêtes asynchrones.

   ```javascript
   // JavaScript
   const xhr = new XMLHttpRequest();
   xhr.open('GET', 'https://api.example.com/data', true);
   xhr.onreadystatechange = function() {
       if (xhr.readyState === 4 && xhr.status === 200) {
           const responseData = JSON.parse(xhr.responseText);
           // Traitez les données de la réponse ici
       }
   };
   xhr.send();
   ```

#### 5.2 AJAX avec jQuery
   - Utilisation de la fonction `$.ajax()` pour effectuer des requêtes asynchrones avec jQuery.

   ```javascript
   // jQuery
   $.ajax({
       url: 'https://api.example.com/data',
       method: 'GET',
       dataType: 'json',
       success: function(data) {
           // Traitez les données de la réponse ici
       },
       error: function(error) {
           console.error('Erreur lors de la requête AJAX:', error);
       }
   });
   ```

#### Exercice intégré : Utilisation de l'API BAN avec AJAX

##### Objectif
   - Intégrer l'API BAN pour suggérer des adresses lors de la saisie en utilisant AJAX.

##### Instructions

1. **Création de l'interface HTML**
   - Créez un champ de texte (`input`) pour que l'utilisateur saisisse une adresse.
   - Ajoutez un élément (`ul` ou `div`) pour afficher les suggestions d'adresses.

   ```html
   <!-- HTML -->
   <input type="text" id="adresseInput" placeholder="Saisissez une adresse">
   <ul id="suggestionsList"></ul>
   ```

2. **Récupération des suggestions avec AJAX**
   - Utilisez AJAX pour détecter les changements dans le champ de texte.
   - À chaque changement, appelez l'API BAN pour obtenir des suggestions d'adresses.

   ```javascript
   // JavaScript avec AJAX
   const adresseInput = document.getElementById('adresseInput');
   const suggestionsList = document.getElementById('suggestionsList');

   adresseInput.addEventListener('input', function() {
       const inputValue = adresseInput.value;

       const xhr = new XMLHttpRequest();
       xhr.open('GET', `https://api-adresse.data.gouv.fr/search/?q=${inputValue}`, true);
       xhr.onreadystatechange = function() {
           if (xhr.readyState === 4 && xhr.status === 200) {
               const data = JSON.parse(xhr.responseText);
               const suggestions = data.features;
               afficherSuggestions(suggestions);
           }
       };
       xhr.send();
   });

   function afficherSuggestions(suggestions) {
       suggestionsList.innerHTML = ''; // Efface les anciennes suggestions

       suggestions.forEach(suggestion => {
           const li = document.createElement('li');
           li.textContent = suggestion.properties.label;
           suggestionsList.appendChild(li);
       });
   }
   ```

3. **Utilisation de jQuery pour AJAX (optionnel)**
   - Si vous préférez utiliser jQuery, vous pouvez également effectuer ces opérations avec jQuery.

   ```javascript
   // jQuery avec AJAX
   $(document).ready(function() {
       $('#adresseInput').on('input', function() {
           const inputValue = $(this).val();

           $.ajax({
               url: `https://api-adresse.data.gouv.fr/search/?q=${inputValue}`,
               method: 'GET',
               dataType: 'json',
               success: function(data) {
                   const suggestions = data.features;
                   afficherSuggestions(suggestions);
               },
               error: function(error) {
                   console.error('Erreur lors de la récupération des suggestions:', error);
               }
           });
       });
   });
   ```

### Ressources additionnelles

#### Cours W3Schools sur le DOM JavaScript
   - [W3Schools JavaScript HTML DOM](https://www.w3schools.com/js.asp)

---



# Cours sur le DOM et le BOM en JavaScript

## Introduction :

Le DOM (Document Object Model) et le BOM (Browser Object Model) sont deux interfaces essentielles en JavaScript pour interagir avec les documents HTML et le navigateur lui-même. Dans ce cours, nous allons explorer en détail ces deux modèles, en fournissant des exemples pratiques et en mettant en évidence les bonnes pratiques de développement.

## Le DOM (Document Object Model) :

Le DOM est une interface de programmation d'application qui représente la structure d'un document HTML ou XML sous forme d'un arbre d'objets. Il fournit une manière de manipuler dynamiquement la structure, le style et le contenu d'une page web.

### Sélection des éléments :

Pour interagir avec les éléments HTML d'une page, nous utilisons différentes méthodes de sélection.

#### Méthodes de sélection :

- `document.getElementById()`: Sélectionne un élément par son ID.
- `document.getElementsByClassName()`: Sélectionne des éléments par leur classe.
- `document.getElementsByTagName()`: Sélectionne des éléments par leur balise.
- `document.querySelector()`: Sélectionne le premier élément qui correspond au sélecteur CSS spécifié.
- `document.querySelectorAll()`: Sélectionne tous les éléments qui correspondent au sélecteur CSS spécifié.

Exemple :

```javascript
// Sélection par ID
let elementById = document.getElementById('monElement');

// Sélection par classe
let elementsByClass = document.getElementsByClassName('maClasse');

// Sélection par balise
let elementsByTag = document.getElementsByTagName('div');

// Sélection avec querySelector
let firstElement = document.querySelector('.maClasse');
let allElements = document.querySelectorAll('.maClasse');
```

### Manipulation des éléments :

Une fois que nous avons sélectionné des éléments, nous pouvons les manipuler en modifiant leur contenu, leurs attributs, leurs styles, etc.

#### Manipulation du contenu :

- `innerHTML`: Modifie le contenu HTML d'un élément.
- `innerText` / `textContent`: Modifie le texte d'un élément.
- `createElement()`: Crée un nouvel élément.
- `appendChild()`: Ajoute un élément enfant à un élément parent.

Exemple :

```javascript
// Modification du contenu HTML
element.innerHTML = '<p>Nouveau contenu</p>';

// Modification du texte
element.innerText = 'Nouveau texte';

// Création d'un nouvel élément
let newElement = document.createElement('div');
newElement.innerText = 'Nouvel élément';

// Ajout du nouvel élément
parentElement.appendChild(newElement);
```

#### Manipulation des attributs :

- `getAttribute()`: Obtient la valeur d'un attribut.
- `setAttribute()`: Définit la valeur d'un attribut.
- `removeAttribute()`: Supprime un attribut.

Exemple :

```javascript
// Obtention de la valeur de l'attribut
let value = element.getAttribute('href');

// Définition de la valeur de l'attribut
element.setAttribute('href', 'https://example.com');

// Suppression de l'attribut
element.removeAttribute('href');
```

#### Manipulation des styles :

- `style`: Permet d'accéder aux styles CSS d'un élément.
- `classList`: Permet de gérer les classes CSS d'un élément.

Exemple :

```javascript
// Modification du style
element.style.color = 'red';
element.style.fontSize = '20px';

// Gestion des classes
element.classList.add('nouvelleClasse');
element.classList.remove('ancienneClasse');
element.classList.toggle('classeToggle');
```

### Événements :

En JavaScript, nous pouvons attacher des fonctions aux événements sur les éléments DOM.

#### Ajout d'événements :

- `addEventListener()`: Définit une fonction à exécuter lorsqu'un événement spécifié se produit.

Exemple :

```javascript
element.addEventListener('click', () => {
    // Code à exécuter lors du clic sur l'élément
});
```

### Fonctions traditionnelles vs Fonctions fléchées :

Nous avons le choix entre des fonctions traditionnelles et des fonctions fléchées pour définir des comportements.

#### Fonctions traditionnelles :

```javascript
element.addEventListener('click', function() {
    // Code à exécuter
});
```

#### Fonctions fléchées :

```javascript
element.addEventListener('click', () => {
    // Code à exécuter
});
```

## Bonnes pratiques :

- Utilisez des sélecteurs spécifiques pour éviter les recherches coûteuses.
- Stockez les sélections DOM dans des variables pour une utilisation répétée.
- Séparez la structure HTML, le style CSS et le comportement JavaScript.
- Utilisez des classes CSS pour la manipulation des styles plutôt que de les modifier directement.

## Le BOM (Browser Object Model) :

Le BOM est une interface pour interagir avec le navigateur web. Il fournit des objets tels que `window`, `document`, `navigator`, etc.

### Objets principaux du BOM :

- `window`: Représente la fenêtre du navigateur.
- `document`: Représente le document chargé dans la fenêtre du navigateur.
- `navigator`: Fournit des informations sur le navigateur de l'utilisateur.
- `location`: Fournit des informations sur l'URL du document en cours.

Exemple :

```javascript
// Ouvrir une nouvelle fenêtre
window.open('https://example.com');

// Rediriger vers une nouvelle URL
window.location.href = 'https://example.com';

// Afficher des informations sur le navigateur
console.log(navigator.userAgent);

// Obtenir l'URL du document
console.log(location.href);
```
