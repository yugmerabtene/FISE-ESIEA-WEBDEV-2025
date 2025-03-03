### Table des matières du syllabus :
1. **Introduction à Node.js**
2. **Installation et configuration de Node.js**
3. **Création d'un serveur Node.js simple**
4. **Gestion des dépendances avec npm**
5. **Introduction à React.js**
6. **Installation et configuration de React.js**
7. **Création d'un composant React simple**
8. **Gestion de l'état avec React (useState, useEffect)**
9. **Communication entre Node.js et React (API REST)**
10. **Exercice final : Création d'une mini-application de gestion de tâches**
    - a. **Backend avec Node.js et Express**
    - b. **Frontend avec React.js**

---

### 1. Introduction à Node.js

**Node.js** est un environnement d'exécution JavaScript côté serveur. Il permet d'exécuter JavaScript sur le serveur, ce qui permet de créer des applications web performantes et non bloquantes. Node.js utilise le moteur JavaScript V8 de Google et est particulièrement adapté aux applications en temps réel, comme les chats ou les applications de collaboration.

### 2. Installation et configuration de Node.js

1. **Installer Node.js** :
   - Allez sur [nodejs.org](https://nodejs.org/) et téléchargez la version stable (LTS) de Node.js.
   - Une fois installé, vérifiez l'installation en exécutant les commandes suivantes dans le terminal :
   
   ```bash
   node -v
   npm -v
   ```

   Ces commandes devraient afficher les versions de Node.js et npm.

2. **Initialiser un projet Node.js** :
   - Créez un nouveau dossier pour votre projet.
   - Initialisez un projet Node.js avec npm :
   
   ```bash
   npm init -y
   ```

   Cela crée un fichier `package.json` qui contient les informations et dépendances de votre projet.

### 3. Création d'un serveur Node.js simple

Vous pouvez créer un serveur HTTP de base avec Node.js en utilisant le module `http`.

```javascript
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello, World!');
});

server.listen(3000, 'localhost', () => {
    console.log('Server running at http://localhost:3000/');
});
```

- Pour démarrer le serveur, exécutez `node server.js` dans le terminal.
- Ouvrez votre navigateur et allez sur `http://localhost:3000/` pour voir la réponse "Hello, World!".

### 4. Gestion des dépendances avec npm

1. **Installer des packages** :
   - Utilisez npm pour installer des packages externes. Par exemple, pour installer **Express.js**, qui simplifie la création de serveurs HTTP avec Node.js, exécutez :

   ```bash
   npm install express
   ```

2. **Utiliser Express pour créer un serveur simple** :

```javascript
// server.js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello, Express!');
});

app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
});
```

### 5. Introduction à React.js

**React.js** est une bibliothèque JavaScript développée par Facebook pour la création d'interfaces utilisateur. Elle permet de construire des interfaces dynamiques en utilisant des composants réutilisables et un état interne. React est basé sur le concept de **Virtual DOM**, ce qui le rend très performant.

### 6. Installation et configuration de React.js

1. **Installer Create React App** :
   - `Create React App` est un outil qui configure un projet React avec toutes les configurations par défaut.
   - Exécutez la commande suivante pour créer un nouveau projet React :

   ```bash
   npx create-react-app my-app
   ```

2. **Lancer l'application React** :
   - Allez dans le dossier de votre projet et lancez le serveur de développement :

   ```bash
   cd my-app
   npm start
   ```

   Votre application React sera accessible à l'adresse `http://localhost:3000`.

### 7. Création d'un composant React simple

Un composant React est une fonction ou une classe qui retourne un élément JSX (une syntaxe qui ressemble à du HTML). Voici un exemple de composant React simple :

```javascript
// src/components/Hello.js
import React from 'react';

function Hello() {
  return <h1>Hello, React!</h1>;
}

export default Hello;
```

Pour utiliser ce composant, modifiez le fichier `src/App.js` :

```javascript
// src/App.js
import React from 'react';
import Hello from './components/Hello';

function App() {
  return (
    <div>
      <Hello />
    </div>
  );
}

export default App;
```

### 8. Gestion de l'état avec React (useState, useEffect)

React permet de gérer l'état avec le hook `useState` et d'effectuer des effets de bord avec le hook `useEffect`.

#### a. Exemple de `useState` :

```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

#### b. Exemple de `useEffect` :

```javascript
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(seconds => seconds + 1);
    }, 1000);

    return () => clearInterval(interval); // Cleanup on unmount
  }, []);

  return <p>Timer: {seconds} seconds</p>;
}

export default Timer;
```

### 9. Communication entre Node.js et React (API REST)

Pour permettre à React d'interagir avec Node.js, nous allons créer une API REST dans Node.js et récupérer les données dans notre application React.

#### a. Créer une API REST avec Express

```javascript
// server.js (Node.js)
const express = require('express');
const app = express();
const cors = require('cors');

app.use(cors());

app.get('/api/tasks', (req, res) => {
  const tasks = [
    { id: 1, name: 'Learn React' },
    { id: 2, name: 'Build a web app' }
  ];
  res.json(tasks);
});

app.listen(3001, () => {
  console.log('API running on http://localhost:3001');
});
```

#### b. Récupérer les données dans React avec `fetch`

```javascript
import React, { useState, useEffect } from 'react';

function TaskList() {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    fetch('http://localhost:3001/api/tasks')
      .then(response => response.json())
      .then(data => setTasks(data));
  }, []);

  return (
    <div>
      <h1>Tasks</h1>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>{task.name}</li>
        ))}
      </ul>
    </div>
  );
}

export default TaskList;
```

### 10. Exercice final : Création d'une mini-application de gestion de tâches

L'objectif de cet exercice est de créer une application où l'utilisateur peut afficher, ajouter, et supprimer des tâches.

#### a. **Backend avec Node.js et Express**

1. Créez une API REST avec Node.js qui permet de récupérer, ajouter et supprimer des tâches.
2. Utilisez une base de données temporaire en mémoire ou un fichier JSON pour stocker les tâches.

#### b. **Frontend avec React.js**

1. Créez un composant `TaskList` pour afficher la liste des tâches.
2. Créez un composant `AddTaskForm` pour permettre à l'utilisateur d'ajouter de nouvelles tâches.
3. Utilisez `fetch` pour récupérer et envoyer des données à votre serveur Node.js.
4. Implémentez un bouton "Supprimer" pour chaque tâche, qui envoie une requête DELETE au serveur.

##### Backend (Node.js) :

```javascript
// server.js (Node.js)
const express = require('express');
const app = express();
const cors = require('cors');
app.use(cors());
app.use(express.json());

let tasks = [
  { id: 1, name: 'Learn React' },
  { id: 2, name: 'Build a web app' }
];

app.get('/api/tasks', (req, res) => {
  res.json(tasks);
});

app.post('/api/tasks', (req, res) => {
  const newTask = req.body;
  tasks.push(newTask);
  res.status(201).json(newTask);
});

app.delete('/api/tasks/:id', (req, res) => {
  tasks = tasks.filter(task => task.id !== parseInt(req.params.id));
  res.status(204).send();
});

app.listen(3001, () => {
  console.log('API running on http://localhost:3001');
});
```

##### Frontend (React) :

1. Créez un composant `TaskList` pour afficher les tâches.
2. Créez un composant `AddTaskForm` pour ajouter une tâche.

```javascript
// TaskList.js (React)
import React, { useState, useEffect } from 'react';

function TaskList() {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    fetch('http://localhost:3001/api/tasks')
      .then(response => response.json())
      .then(data => setTasks(data));
  }, []);

  const deleteTask = id => {
    fetch(`http://localhost:3001/api/tasks/${id}`, {
      method: 'DELETE',
    }).then(() => {
      setTasks(tasks.filter(task => task.id !== id));
    });
  };

  return (
    <div>
      <h1>Task List</h1>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>
            {task.name} 
            <button onClick={() => deleteTask(task.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TaskList;
```
