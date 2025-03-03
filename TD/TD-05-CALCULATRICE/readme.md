### Énoncé :
Vous devez créer une calculatrice numérique simple avec une interface utilisateur graphique. La calculatrice doit répondre aux exigences suivantes :
- **Fonctionnalités :**
  - Afficher les chiffres et les opérations en cours dans une zone d’affichage.
  - Permettre l’entrée des chiffres de 0 à 9 et d’un point décimal (.).
  - Inclure les opérations de base : addition (+), soustraction (-), multiplication (×), division (÷).
  - Ajouter un bouton pour effacer l’affichage (C) et un bouton pour calculer le résultat (=).
  - Gérer les erreurs simples (ex. : division par zéro) en affichant "Erreur" temporairement.
- **Interface :**
  - Utiliser HTML pour la structure et Flexbox en CSS pour une disposition en grille des boutons.
  - Centrer la calculatrice sur la page avec un design moderne (ombres, couleurs distinctes pour les opérateurs, etc.).
  - Assurer une mise en page responsive.
- **Contraintes :**
  - Séparer le code en trois fichiers : HTML, CSS, et JavaScript.
  - Ne pas utiliser `console.log` pour l’affichage.

---

### 1. `index.html`
```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculatrice</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button class="operator" onclick="appendToDisplay('/')">÷</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button onclick="appendToDisplay('.')">.</button>
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button onclick="appendToDisplay('0')">0</button>
            <button class="equals" onclick="calculate()">=</button>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

---

### 2. `styles.css`
```css
body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f0f0f0;
    font-family: Arial, sans-serif;
}

.calculator {
    background-color: #333;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}

.display {
    width: 100%;
    height: 60px;
    background-color: #fff;
    margin-bottom: 10px;
    border-radius: 5px;
    display: flex;
    align-items: center;
    justify-content: flex-end;
    padding: 0 10px;
    font-size: 24px;
    box-sizing: border-box;
}

.buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 5px;
    width: 240px;
}

button {
    flex: 1 1 22%;
    height: 50px;
    font-size: 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    background-color: #fff;
    transition: background-color 0.2s;
}

button:hover {
    background-color: #ddd;
}

.operator {
    background-color: #f39c12;
    color: white;
}

.operator:hover {
    background-color: #e67e22;
}

.equals {
    background-color: #2ecc71;
    color: white;
}

.equals:hover {
    background-color: #27ae60;
}

.clear {
    background-color: #e74c3c;
    color: white;
}

.clear:hover {
    background-color: #c0392b;
}
```

---

### 3. `script.js`
```javascript
let display = document.getElementById('display');
let currentInput = '0';

function appendToDisplay(value) {
    if (currentInput === '0' && value !== '.') {
        currentInput = value;
    } else {
        currentInput += value;
    }
    display.textContent = currentInput;
}

function clearDisplay() {
    currentInput = '0';
    display.textContent = currentInput;
}

function calculate() {
    try {
        currentInput = eval(currentInput.replace('×', '*').replace('÷', '/')).toString();
        display.textContent = currentInput;
    } catch (error) {
        currentInput = 'Erreur';
        display.textContent = currentInput;
        setTimeout(clearDisplay, 1000);
    }
}
```

---

### Instructions pour tester :
1. Crée un dossier pour ton projet.
2. Ajoute ces trois fichiers (`index.html`, `styles.css`, `script.js`) dans ce dossier.
3. Ouvre `index.html` dans un navigateur.

### Résultat attendu :
- Une calculatrice centrée avec une grille de boutons (4x4).
- Les chiffres s’affichent correctement, les opérations fonctionnent, et les erreurs sont gérées avec un message temporaire "Erreur".
- Le design est moderne et responsive grâce à Flexbox.
