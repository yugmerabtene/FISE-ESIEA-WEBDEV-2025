### DOM
Le **Document Object Model (DOM)** est un ensemble d'outils permettant de manipuler des documents XML (et, par extension, également des documents HTML, qui sont probablement ceux qui nous intéressent le plus).

Avant de pouvoir utiliser ces outils et aides, le DOM transforme les fichiers XML en une structure arborescente, c'est-à-dire une hiérarchie de nœuds. Cet arbre représente à la fois le contenu du document et les relations entre les nœuds.

![http://www.w3schools.com/js/pic_htmltree.gif](http://www.w3schools.com/js/pic_htmltree.gif)

#### Types de nœuds
Notez comment, dans l'exemple ci-dessus, nous avons différents types de nœuds : la plupart sont étiquetés comme **Element**, mais d'autres sont marqués comme **Attribute** ou **Text**. Le DOM définit différents types de nœuds qui implémentent différentes interfaces. Voici une liste des types de nœuds les plus courants :

- **Document** : le nœud racine de tous les documents XML (HTML).
- **DocumentType** : représente la définition du type de document (DTD) utilisée sur la page (balise doctype).
- **Element** : représente une balise.
- **Attr** : une paire clé-valeur représentant un attribut d'une balise.
- **Text** : représente le contenu d'un nœud.
- **Comment** : représente les commentaires XML/HTML.

#### L'interface Node
Un objet **Node** implémente et expose les propriétés et méthodes suivantes. Vous pouvez trouver une liste complète sur [MDN](https://developer.mozilla.org).

- `nodeName`
- `nodeValue`
- `nodeType`
- `ownerDocument`
- `firstChild`
- `lastChild`
- `childNodes`
- `previousSibling`
- `nextSibling`
- `hasChildNodes()`
- `attributes`
- `parentNode`
- `parentElement`
- `appendChild(node)`
- `removeChild(node)`
- `replaceChild(newNode, previousNode)`
- `insertBefore(newNode, previousNode)`

#### Accéder aux nœuds
La manière la plus simple d'accéder aux nœuds est de les référencer directement en utilisant l'une des méthodes suivantes :

- `document.querySelector(selector)` : retourne un **Element**. Le sélecteur peut être quelque chose comme `#some-id`, `.some-classname`, ou une balise. S'il y a plusieurs éléments correspondant au sélecteur, seul le premier est retourné.
- `document.querySelectorAll(selector)` : retourne une **NodeList**.
- `document.getElementById(id: string)` : retourne un **Element**.
- `document.getElementsByClassName(classname: string)` : retourne une **HTMLCollection**.
- `document.getElementsByTagName(tag: string)` : retourne une **HTMLCollection**.
- `document.getElementsByName(name: string)` : retourne une **NodeList**.

Nous pouvons également utiliser la plupart de ces méthodes (sauf `getElementById` et `getElementByName`) sur n'importe quel nœud de type **Element**, qui agit comme l'élément racine de notre requête :

```javascript
const wrapper = document.querySelector('#wrapper');
wrapper.getElementsByTagName('p');
wrapper.getElementsByClassName('active');
wrapper.getElementsByName('something');
```

Remarquez que certaines méthodes renvoient une **NodeList**, tandis que d'autres renvoient une **HTMLCollection**. Une **HTMLCollection** est toujours dynamique, ce qui signifie que les changements dans le DOM sont reflétés dans la collection. Une **NodeList**, en revanche, peut être statique ou dynamique. `document.querySelectorAll` renvoie une **NodeList** statique, ce qui signifie que les modifications ultérieures du DOM ne seront pas reflétées dans l'objet **NodeList** statique.

**Note sur les performances** : dans certains navigateurs, l'utilisation de `querySelectorAll` entraîne des coûts de performance significatifs par rapport à, par exemple, `getElementsByTagName`. Cela est dû au fait qu'une **NodeList** statique doit avoir toutes les données nécessaires à l'avance, alors qu'une **NodeList** dynamique peut être générée et retournée beaucoup plus rapidement.

Poursuivons… Nous pouvons également utiliser la plupart des propriétés de l'interface **Node** pour parcourir l'objet document en spécifiant le chemin jusqu'au nœud que nous essayons d'atteindre. Par exemple :

```javascript
document.documentElement.lastChild;
document.documentElement.childNodes[0];
document.documentElement.firstChild.nextSibling;
document.body.previousSibling;
document.body.parentNode;
```

**Remarque annexe** : dans la plupart des cas, `parentNode` et `parentElement` renvoient la même chose. Ils diffèrent uniquement lorsque le nœud parent n'est pas de type **Element**, auquel cas `parentElement` renverra `null`. Par exemple :

```javascript
// les deux renvoient l'élément html
document.body.parentNode === document.body.parentElement; // true
document.documentElement.parentNode; // renvoie le nœud document
document.documentElement.parentElement; // renvoie null
```

**Autre remarque rapide** : sur la différence entre `document` et `document.documentElement`. Ce dernier renvoie l'élément `html`, ce qui est probablement ce que vous cherchez si vous essayez de parcourir le DOM. De plus, ils ont des types de nœuds différents :

```javascript
document.nodeType; // 9 = DOCUMENT_NODE
document.documentElement.nodeType; // 1 = ELEMENT_NODE
```

#### Attributs
Les nœuds DOM de type **Element** exposent une propriété `attributes` de type **NamedNodeMap**, qui est une collection de tous les attributs d'un élément particulier.

```javascript
const element = document.querySelector('#title');
const className = element.attributes.class; // notation par point
const firstAttribute = element.attributes[0]; // notation par tableau
```

Notez que tout élément dans la collection `attributes` est un objet de type **Node** pour lequel `nodeType = 2`, c'est-à-dire : **ATTRIBUTE_NODE**.

```javascript
element.attributes.class.nodeType; // renvoie 2 = ATTRIBUTE_NODE
```

Nous pouvons obtenir, définir et supprimer des attributs en utilisant ces méthodes :

```javascript
element.getAttribute('class');
element.setAttribute('class', 'new-classname');
element.setAttributeNode(attributeNode);
element.removeAttribute('class');
```

La spécification DOM HTML permet d'accéder et de modifier tous les attributs directement en assignant de nouvelles valeurs :

```html
<img src="image.jpg" alt="Ceci est une image" class="logo logo-sm" />
```
```javascript
const image = document.querySelector('.logo');
image.src; // renvoie "image.jpg"
image.alt = "Ceci est une image GÉNIALE"; // met à jour le texte alternatif
```

Cela est valable pour tous les attributs sauf `class`. En effet, `class` est un mot-clé réservé utilisé depuis ES6. C'est pourquoi nous devons utiliser le nom alternatif `className`. Il en va de même pour l'attribut `for` dans l'élément `label`, par exemple : nous utilisons `htmlFor` à la place.

```javascript
image.className; // renvoie "logo logo-sm"
image.class; // renvoie undefined
```

Pour les classes CSS, nous avons également accès à la propriété `classList`, qui est une collection de type **DOMTokenList** listant tous les noms de classes de l'élément. Cette interface implémente quatre méthodes importantes : `add`, `remove`, `toggle` et `contains`.

```javascript
image.classList; // renvoie ["logo", "logo-sm"]
image.classList.add('logo-awesome');
image.classList.remove('logo-sm');
image.classList.toggle('active');
image.classList.contains('this-class-doesnt-exist');
image.classList; // renvoie maintenant ["logo", "logo-awesome", "active"]
```

Une dernière chose, cette fois sur la liste de toutes les propriétés CSS calculées d'un élément. Il existe une méthode appelée `window.getComputedStyles(element)` à laquelle vous passez votre élément, et elle renvoie un énorme objet de type **CSSStyleDeclaration** que vous pouvez utiliser pour déterminer les styles calculés d'un élément, peu importe comment ils ont été appliqués.

```javascript
getComputedStyle(image).width; // renvoie la largeur réelle de l'image
```

#### Créer, supprimer et remplacer des nœuds
Il existe différentes méthodes d'usine attachées à l'objet `document` que nous pouvons utiliser pour créer des nœuds de différents types. Les plus courantes sont :

- `document.createElement(tag: string)` : **Element**
- `document.createAttribute(name: string)` : **Attr**
- `document.createTextNode(text: string)` : **Text**
- `document.createComment(comment: string)` : **Comment**

Quelques exemples :

```javascript
const p = document.createElement('p');
const blurb = document.createTextNode('salut mec');
const attr = document.createAttribute('class');
p.appendChild(blurb);
attr.value = 'my-custom-class';
p.setAttributeNode(attr);
p.setAttribute('is-awesome', true);
```

Pour supprimer des nœuds, utilisez la méthode `removeChild` en interrogeant d'abord le parent. `removeChild` prend comme paramètre l'élément que nous essayons de supprimer.

Il existe également la méthode `remove` que nous pouvons utiliser sur les éléments, bien qu'elle ne soit pas prise en charge sur Internet Explorer et nécessite un polyfill.

```javascript
const element = document.querySelector('#title');
element.parentNode.removeChild(element);
element.remove(); // ceci ne fonctionne pas sur IE !
```

Nous pouvons remplacer un nœud via la méthode `replaceChild`. C'est un processus similaire à celui utilisé pour supprimer un nœud : nous interrogeons d'abord le nœud parent, puis nous appelons la méthode `replaceChild` sur le parent en passant le nouvel élément en premier et l'ancien élément (celui que nous voulons remplacer) en second paramètre, et nous récupérons le nœud remplacé en retour.

```javascript
const oldElement = document.querySelector('#title');
const newElement = document.createElement('p');
const newElementText = document.createTextNode('notre nouveau titre');
newElement.appendChild(newElementText);
const replacedNode = oldElement.parentNode.replaceChild(newElement, oldElement);
replacedNode === oldElement; // true
```

#### Relations entre nœuds
Il existe plusieurs façons d'établir une relation entre deux nœuds :

- `element.append`
- `element.appendChild`
- `element.insertBefore`
- `element.insertAdjacentElement`

#### Événements
Chaque élément HTML a sa propre liste de types d'événements pris en charge. Nous écoutons les événements sur un élément, et chaque fois que l'événement est déclenché, nous le gérons via une fonction appelée **gestionnaire d'événements**.

Un élément peut écouter et répondre à plusieurs types d'événements.
Un type d'événement peut être géré par plusieurs éléments.

Vous pouvez lier des gestionnaires d'événements aux éléments via la méthode `addEventListener`. Le premier argument est le nom de l'événement, le second est le gestionnaire d'événements :

```javascript
const button = document.querySelector('#button');
button.addEventListener('click', handleClick);
function handleClick(event) {
  console.log('cliqué');
}
```

Notre fonction de gestion d'événements reçoit un objet événement passé en paramètre.

Pour arrêter d'écouter un événement sur un élément particulier, utilisez la méthode `removeEventListener` sur celui-ci :

```javascript
button.removeEventListener('click', handleClick);
```

Il y a toutefois une subtilité. Pour supprimer des gestionnaires d'événements, la fonction de gestion doit être une fonction nommée, et vous devez conserver une référence à la fonction qui gère l'événement. En d'autres termes, les fonctions anonymes ne fonctionneront pas si vous prévoyez de supprimer l'écouteur plus tard.

Voici une liste de certains des types d'événements les plus utilisés :

- **Événements de souris** : `click`, `dblclick`, `mousedown`, `mouseout`, `mouseover`, `mouseup`, `mousemove`.
- **Événements de clavier** : `keydown`, `keypress`, `keyup`.
- **Événements HTML** : `load`, `unload`, `abort`, `error`, `resize`, `change`, `submit`, `reset`, `scroll`, `focus`, `blur`.
- **Événements DOM** : `DOMSubtreeModified`, `DOMNodeInserted`, `DOMNodeRemoved`.

Pour une liste plus complète des types d'événements, consultez [ce lien](lien).

### BOM
Le **Browser Object Model (BOM)** permet à JavaScript de communiquer avec le navigateur sur des sujets autres que le contenu de la page.

Il n'existe pas de normes officielles pour le BOM, bien que les fournisseurs de navigateurs aient implémenté presque les mêmes fonctionnalités pour assurer l'interopérabilité.

![https://learn.javascript.ru/article/browser-environment/windowObjects.png](https://learn.javascript.ru/article/browser-environment/windowObjects.png)

#### Variables et méthodes globales
L'objet `window` est l'élément racine ultime, tout le reste y est attaches directement ou indirectement. Il n'est donc pas nécessaire de le référencer explicitement.

Dans les navigateurs à onglets, chaque onglet possède son propre objet `window`, c'est-à-dire qu'il n'est pas partagé entre les onglets d'une même fenêtre.

De plus, toutes les variables et fonctions qui ne sont pas attachées à un autre objet sont attachées à l'objet `window` et sont donc dans la portée globale. En d'autres termes : les variables globales deviennent des propriétés de l'objet `window`, tandis que les fonctions globales deviennent des méthodes de l'objet `window`.

L'objet global `window` donne accès à :

- **Propriétés** qui fournissent des informations sur la fenêtre du navigateur :
```javascript
// dimensions extérieures
const outerHeight = window.outerHeight;
const outerWidth = window.outerWidth;
// dimensions intérieures
const innerHeight = window.innerHeight;
const innerWidth = window.innerWidth;
```

- **Méthodes** pour définir des minuteries et appeler une fonction de manière répétée :
```javascript
const timeout = setTimeout(callback, delay); // délai en ms
const interval = setInterval(callback, delay); // délai en ms
clearTimeout(timeout);
clearInterval(interval);
```

Et bien d'autres. Voici une liste complète de toutes les propriétés et méthodes de l'objet global `window`.

#### L'objet location
En raison de l'absence de norme, l'objet `location` est attaché à la fois à `document` et à `window` :

```javascript
document.location === window.location; // renvoie true
```

Cet objet permet de lire et de manipuler l'URL dans la barre d'adresse du navigateur. Voici une liste des principales propriétés et méthodes qu'il expose (une liste plus complète est disponible [ici](lien)) :

- `location.hash`
- `location.host`
- `location.hostname`
- `location.href` : renvoie l'URL complète de la page actuelle. Nous pouvons également écrire dans cette propriété, provoquant une redirection vers la nouvelle valeur.
- `location.pathname` : renvoie ce qui suit le nom d'hôte.
- `location.port` : renvoie le numéro de port, mais uniquement s'il est défini dans l'URL.
- `location.protocol` : renvoie le protocole utilisé pour accéder à la page.
- `location.search` : renvoie ce qui suit le `?` dans l'URL.
- `location.assign(url)` : navigue vers l'URL passée en paramètre.
- `location.replace(url)` : similaire à `assign`, mais le site remplacé est retiré de l'historique de la session.
- `location.reload()` : a le même effet que cliquer sur le bouton de rechargement du navigateur.

Plus d'informations sur l'interface `Location` sur [MDN](https://developer.mozilla.org).

#### L'objet history
`window.history` permet de manipuler l'historique de la session du navigateur, c'est-à-dire les pages visitées dans l'onglet ou le cadre où la page actuelle est chargée.

- `history.length` : renvoie un entier représentant le nombre d'éléments dans l'historique de la session, y compris la page actuellement chargée.
- `history.go(integer)` : redirige vers la page dans l'historique de la session identifiée par sa position relative par rapport à la page actuelle.
- `history.back(integer)` : redirige vers la page précédente dans l'historique de la session, comme cliquer sur le bouton "Retour" du navigateur.
- `history.forward(integer)` : redirige vers la page suivante dans l'historique de la session, comme cliquer sur le bouton "Avancer" du navigateur.

Plus d'informations sur l'interface `History` [ici](lien).

#### L'objet navigator
`window.navigator` fournit des informations sur l'état et l'identité de l'agent utilisateur (c'est-à-dire le navigateur et le système d'exploitation utilisés par l'utilisateur).

- `navigator.userAgent` : renvoie l'agent utilisateur pour le navigateur actuel.
- `navigator.language` : renvoie une chaîne représentant la langue préférée de l'utilisateur, généralement la langue de l'interface du navigateur.
- `navigator.languages` : renvoie un tableau des langues connues de l'utilisateur, triées par préférence : `["en-GB", "en-US", "en"]`.
- `navigator.getBattery()` : renvoie une promesse qui se résout en un objet `BatteryManager` fournissant non seulement des informations sur la batterie du système, mais aussi des événements pour surveiller son état.
- `navigator.plugins` : renvoie une liste des plugins installés dans le navigateur.
- `navigator.onLine` : renvoie un booléen indiquant si le navigateur fonctionne en ligne.
- `navigator.geolocation` : renvoie un objet `Geolocation` permettant d'accéder à la position de l'appareil.

Il existe également d'autres propriétés sur lesquelles nous ne pouvons pas toujours compter :

- `navigator.appName` : devrait renvoyer le nom du navigateur.
- `navigator.appVersion` : devrait renvoyer le numéro de version du navigateur.
- `navigator.platform` : devrait renvoyer le nom de la plateforme du navigateur.
