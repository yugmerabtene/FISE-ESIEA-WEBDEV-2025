### **Cahier des charges**

#### **Objectif**
Développer un système de gestion d’utilisateurs utilisant **PHP orienté objet (POO)** avec une **architecture MVC**, permettant :
- L’inscription avec nom, prénom, email (unique), adresse (via API BAN : https://api.gouv.fr/les-api/base-adresse-nationale), mot de passe (saisi deux fois, min. 12 caractères avec majuscule, minuscule, chiffre, caractère spécial).
- La connexion sécurisée via email et mot de passe, redirigeant vers une page de profil.
- La modification des informations personnelles dans le profil.
- Gestion centralisée des erreurs : toutes les exceptions sont propagées au point d’entrée (`index.php`) et affichées sur la page d’accueil.

#### **Fonctionnalités principales**
1. **Inscription** :
   - Formulaire avec nom, prénom, email (unique), adresse (autocomplétée via API BAN), mot de passe (vérifié avec regex), et confirmation.
2. **Connexion** :
   - Authentification via email et mot de passe avec redirection vers `/profile` uniquement si succès.
3. **Profil** :
   - Affichage et modification des informations personnelles (nom, prénom, email, adresse).
4. **Sécurité** :
   - Protection contre CSRF (token), XSS (échappement), injection SQL (requêtes préparées).
5. **Gestion des erreurs** :
   - Try-catch dans le modèle et les contrôleurs, exceptions propagées à `index.php` pour affichage sur `home.php`.
6. **Base de données** :
   - Table `users` : id, nom, prénom, email (unique), adresse, mot_de_passe (hashé).

#### **Technologies**
- **Backend** : PHP orienté objet (POO) avec architecture MVC, MySQL.
- **Frontend** : HTML, CSS, JavaScript (fetch pour l’API BAN : `https://api-adresse.data.gouv.fr/search/`).
- **API externe** : BAN (documentation : `https://api.gouv.fr/les-api/base-adresse-nationale`).

---

### **User Stories**

1. **En tant qu’utilisateur non inscrit**, je veux pouvoir m’inscrire avec mon nom, prénom, email, adresse et mot de passe afin de créer un compte.
   - **Critères d’acceptation** :
     - Formulaire avec nom, prénom, email (unique), adresse (autocomplétée), mot de passe (min. 12 caractères, majuscule, minuscule, chiffre, caractère spécial), et confirmation.
     - Erreur affichée sur la page d’accueil si échec.

2. **En tant qu’utilisateur inscrit**, je veux me connecter avec mon email et mot de passe pour accéder à mon profil.
   - **Critères d’acceptation** :
     - Connexion réussie redirige vers `/profile`.
     - Erreur affichée sur la page d’accueil si échec.

3. **En tant qu’utilisateur connecté**, je veux voir et modifier mes informations personnelles sur ma page de profil.
   - **Critères d’acceptation** :
     - Champs nom, prénom, email, adresse pré-remplis et modifiables.
     - Erreur affichée sur la page d’accueil si échec.

4. **En tant qu’administrateur du système**, je veux que les erreurs soient gérées centralement et affichées à l’utilisateur.
   - **Critères d’acceptation** :
     - Toutes les exceptions sont propagées à `index.php` et affichées sur `home.php`.

---

### **Cas d’acceptation détaillés (en français)**

#### **Inscription**
- **Scénario 1 : Inscription réussie**
  - **Étant donné** : Un formulaire rempli (nom : "Jean", prénom : "Dupont", email : "jean.dupont@example.com", adresse : "10 rue de la Paix, Paris", mot de passe : "Motdepasse123!", confirmation : "Motdepasse123!").
  - **Quand** : L’utilisateur clique sur "S’inscrire".
  - **Alors** : Le compte est créé, et l’utilisateur est redirigé vers `/login`.

- **Scénario 2 : Mot de passe invalide**
  - **Étant donné** : Un mot de passe "Motdepasse!" (pas de chiffre).
  - **Quand** : L’utilisateur soumet le formulaire.
  - **Alors** : Une erreur est affichée sur la page d’accueil : "Le mot de passe doit contenir au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial."

- **Scénario 3 : Email déjà utilisé**
  - **Étant donné** : Un email "jean.dupont@example.com" qui existe déjà.
  - **Quand** : L’utilisateur soumet le formulaire avec cet email.
  - **Alors** : Une erreur est affichée sur la page d’accueil : "Cet email est déjà utilisé."

#### **Connexion**
- **Scénario 1 : Connexion réussie**
  - **Étant donné** : Des identifiants corrects (email : "jean.dupont@example.com", mot de passe : "Motdepasse123!").
  - **Quand** : L’utilisateur clique sur "Se connecter".
  - **Alors** : L’utilisateur est redirigé vers `/profile`.

- **Scénario 2 : Connexion échouée**
  - **Étant donné** : Un email ou mot de passe incorrect (ex. mot de passe : "Mauvais123!").
  - **Quand** : L’utilisateur clique sur "Se connecter".
  - **Alors** : Une erreur est affichée sur la page d’accueil : "Email ou mot de passe incorrect."

#### **Modification profil**
- **Scénario 1 : Mise à jour réussie**
  - **Étant donné** : Un email modifié de "jean.dupont@example.com" à "marc.dupont@example.com".
  - **Quand** : L’utilisateur clique sur "Enregistrer".
  - **Alors** : Les données sont mises à jour, et l’utilisateur reste sur `/profile`.

- **Scénario 2 : Email déjà pris**
  - **Étant donné** : Un email modifié qui existe déjà (ex. "jean.dupont@example.com").
  - **Quand** : L’utilisateur clique sur "Enregistrer".
  - **Alors** : Une erreur est affichée sur la page d’accueil : "Cet email est déjà utilisé par un autre utilisateur."

---

### **Structure du projet**

```
/projet
├── /controllers
│   ├── AuthController.php
│   └── ProfileController.php
├── /models
│   └── User.php
├── /views
│   ├── register.php
│   ├── login.php
│   ├── profile.php
│   └── home.php
├── /public
│   ├── index.php
│   ├── style.css
│   └── script.js
├── /config
│   └── database.php
```

---

### **Implémentation complète**

#### **1. Base de données (MySQL)**
```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    adresse VARCHAR(255) NOT NULL,
    mot_de_passe VARCHAR(255) NOT NULL
);
```

#### **2. Configuration (config/database.php)**
```php
<?php
class Database {
    private $pdo;

    public function __construct() {
        try {
            $this->pdo = new PDO("mysql:host=localhost;dbname=gest_users", "root", "");
            $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        } catch (PDOException $e) {
            throw new Exception("Erreur de connexion à la base de données : " . $e->getMessage());
        }
    }

    public function getConnection() {
        return $this->pdo;
    }
}
```

#### **3. Modèle (models/User.php)**
```php
<?php
class User {
    private $db;

    public function __construct() {
        $this->db = (new Database())->getConnection();
    }

    public function register($nom, $prenom, $email, $adresse, $mot_de_passe) {
        if (!preg_match('/^(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[!@#$%^&*]).{12,}$/', $mot_de_passe)) {
            throw new Exception("Le mot de passe doit contenir au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial (ex. !@#$%^&*).");
        }
        if ($this->emailExists($email)) {
            throw new Exception("Cet email est déjà utilisé.");
        }
        try {
            $hash = password_hash($mot_de_passe, PASSWORD_BCRYPT);
            $stmt = $this->db->prepare("INSERT INTO users (nom, prenom, email, adresse, mot_de_passe) VALUES (?, ?, ?, ?, ?)");
            $stmt->execute([$nom, $prenom, $email, $adresse, $hash]);
            return true;
        } catch (PDOException $e) {
            throw new Exception("Erreur lors de l'inscription : " . $e->getMessage());
        }
    }

    public function login($email, $mot_de_passe) {
        try {
            $stmt = $this->db->prepare("SELECT * FROM users WHERE email = ?");
            $stmt->execute([$email]);
            $user = $stmt->fetch(PDO::FETCH_ASSOC);
            if ($user && password_verify($mot_de_passe, $user['mot_de_passe'])) {
                return $user;
            }
            throw new Exception("Email ou mot de passe incorrect.");
        } catch (PDOException $e) {
            throw new Exception("Erreur lors de la connexion : " . $e->getMessage());
        }
    }

    public function update($id, $nom, $prenom, $email, $adresse) {
        try {
            $stmt = $this->db->prepare("SELECT id FROM users WHERE email = ? AND id != ?");
            $stmt->execute([$email, $id]);
            if ($stmt->fetch()) {
                throw new Exception("Cet email est déjà utilisé par un autre utilisateur.");
            }
            $stmt = $this->db->prepare("UPDATE users SET nom = ?, prenom = ?, email = ?, adresse = ? WHERE id = ?");
            $stmt->execute([$nom, $prenom, $email, $adresse, $id]);
            return true;
        } catch (PDOException $e) {
            throw new Exception("Erreur lors de la mise à jour : " . $e->getMessage());
        }
    }

    private function emailExists($email) {
        try {
            $stmt = $this->db->prepare("SELECT id FROM users WHERE email = ?");
            $stmt->execute([$email]);
            return $stmt->fetch() !== false;
        } catch (PDOException $e) {
            throw new Exception("Erreur lors de la vérification de l'email : " . $e->getMessage());
        }
    }

    public function getUserById($id) {
        try {
            $stmt = $this->db->prepare("SELECT * FROM users WHERE id = ?");
            $stmt->execute([$id]);
            return $stmt->fetch(PDO::FETCH_ASSOC);
        } catch (PDOException $e) {
            throw new Exception("Erreur lors de la récupération de l'utilisateur : " . $e->getMessage());
        }
    }
}
```

#### **4. Contrôleur d’authentification (controllers/AuthController.php)**
```php
<?php
session_start();
require_once '../models/User.php';

class AuthController {
    private $user;

    public function __construct() {
        $this->user = new User();
    }

    public function showRegister() {
        $csrf_token = bin2hex(random_bytes(32));
        $_SESSION['csrf_token'] = $csrf_token;
        return '../views/register.php';
    }

    public function register($data) {
        if (!isset($_SESSION['csrf_token']) || $_POST['csrf_token'] !== $_SESSION['csrf_token']) {
            throw new Exception("Erreur CSRF : Token invalide.");
        }
        if ($data['mot_de_passe'] !== $data['mot_de_passe_confirm']) {
            throw new Exception("Les mots de passe ne correspondent pas.");
        }
        $this->user->register($data['nom'], $data['prenom'], $data['email'], $data['adresse'], $data['mot_de_passe']);
        return '/login';
    }

    public function showLogin() {
        return '../views/login.php';
    }

    public function login($data) {
        $user = $this->user->login($data['email'], $data['mot_de_passe']);
        $_SESSION['user_id'] = $user['id'];
        return '/profile';
    }
}
```

#### **5. Contrôleur de profil (controllers/ProfileController.php)**
```php
<?php
session_start();
require_once '../models/User.php';

class ProfileController {
    private $user;

    public function __construct() {
        $this->user = new User();
    }

    public function showProfile() {
        if (!isset($_SESSION['user_id'])) {
            throw new Exception("Vous devez être connecté pour accéder à cette page.");
        }
        $user = $this->user->getUserById($_SESSION['user_id']);
        return ['view' => '../views/profile.php', 'data' => ['user' => $user]];
    }

    public function updateProfile($data) {
        if (!isset($_SESSION['user_id'])) {
            throw new Exception("Vous devez être connecté pour modifier votre profil.");
        }
        $this->user->update($_SESSION['user_id'], $data['nom'], $data['prenom'], $data['email'], $data['adresse']);
        return '/profile';
    }
}
```

#### **6. Vue d’inscription (views/register.php)**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/public/style.css">
    <title>Inscription</title>
</head>
<body>
    <h1>Inscription</h1>
    <form method="POST" action="/register">
        <input type="hidden" name="csrf_token" value="<?php echo $csrf_token; ?>">
        <label>Nom : <input type="text" name="nom" required></label><br>
        <label>Prénom : <input type="text" name="prenom" required></label><br>
        <label>Email : <input type="email" name="email" required></label><br>
        <label>Adresse : <input type="text" id="adresse" name="adresse" required></label>
        <div id="adresse-suggestions"></div><br>
        <label>Mot de passe : <input type="password" name="mot_de_passe" required></label><br>
        <label>Confirmer : <input type="password" name="mot_de_passe_confirm" required></label><br>
        <button type="submit">S'inscrire</button>
    </form>
    <a href="/">Retour à l'accueil</a>
    <script src="/public/script.js"></script>
</body>
</html>
```

#### **7. Vue de connexion (views/login.php)**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/public/style.css">
    <title>Connexion</title>
</head>
<body>
    <h1>Connexion</h1>
    <form method="POST" action="/login">
        <label>Email : <input type="email" name="email" required></label><br>
        <label>Mot de passe : <input type="password" name="mot_de_passe" required></label><br>
        <button type="submit">Se connecter</button>
    </form>
    <a href="/">Retour à l'accueil</a>
</body>
</html>
```

#### **8. Vue de profil (views/profile.php)**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/public/style.css">
    <title>Profil</title>
</head>
<body>
    <h1>Profil</h1>
    <form method="POST" action="/profile/update">
        <label>Nom : <input type="text" name="nom" value="<?php echo htmlspecialchars($user['nom']); ?>" required></label><br>
        <label>Prénom : <input type="text" name="prenom" value="<?php echo htmlspecialchars($user['prenom']); ?>" required></label><br>
        <label>Email : <input type="email" name="email" value="<?php echo htmlspecialchars($user['email']); ?>" required></label><br>
        <label>Adresse : <input type="text" id="adresse" name="adresse" value="<?php echo htmlspecialchars($user['adresse']); ?>" required></label>
        <div id="adresse-suggestions"></div><br>
        <button type="submit">Enregistrer</button>
    </form>
    <a href="/">Retour à l'accueil</a>
    <script src="/public/script.js"></script>
</body>
</html>
```

#### **9. Vue de la page d’accueil (views/home.php)**
```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/public/style.css">
    <title>Accueil</title>
</head>
<body>
    <h1>Bienvenue</h1>
    <?php if (isset($error)): ?>
        <p style="color: red;"><?php echo htmlspecialchars($error); ?></p>
    <?php endif; ?>
    <p><a href="/register">S'inscrire</a></p>
    <p><a href="/login">Se connecter</a></p>
    <?php if (isset($_SESSION['user_id'])): ?>
        <p><a href="/profile">Mon profil</a></p>
    <?php endif; ?>
</body>
</html>
```

#### **10. JavaScript (public/script.js)**
```javascript
document.getElementById('adresse').addEventListener('input', async function(e) {
    const query = e.target.value;
    if (query.length < 3) return;

    const response = await fetch(`https://api-adresse.data.gouv.fr/search/?q=${encodeURIComponent(query)}`);
    const data = await response.json();
    const suggestions = document.getElementById('adresse-suggestions');
    suggestions.innerHTML = '';

    data.features.forEach(feature => {
        const div = document.createElement('div');
        div.textContent = feature.properties.label;
        div.addEventListener('click', () => {
            document.getElementById('adresse').value = feature.properties.label;
            suggestions.innerHTML = '';
        });
        suggestions.appendChild(div);
    });
});
```

#### **11. CSS (public/style.css)**
```css
body { font-family: Arial, sans-serif; padding: 20px; }
form { max-width: 400px; margin: 0 auto; }
label { display: block; margin: 10px 0; }
input { width: 100%; padding: 8px; }
button { background: #007BFF; color: white; padding: 10px; border: none; cursor: pointer; }
#adresse-suggestions { border: 1px solid #ddd; max-height: 150px; overflow-y: auto; }
#adresse-suggestions div { padding: 5px; cursor: pointer; }
#adresse-suggestions div:hover { background: #f0f0f0; }
a { color: #007BFF; text-decoration: none; }
a:hover { text-decoration: underline; }
```

#### **12. Point d’entrée (public/index.php)**
```php
<?php
session_start();
require_once '../controllers/AuthController.php';
require_once '../controllers/ProfileController.php';

$path = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);

try {
    if ($path === '/' || $path === '') {
        require '../views/home.php';
    } elseif ($path === '/register') {
        $controller = new AuthController();
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
            $nextPath = $controller->register($_POST);
            header("Location: $nextPath");
            exit;
        } else {
            $view = $controller->showRegister();
            require $view;
        }
    } elseif ($path === '/login') {
        $controller = new AuthController();
        if ($_SERVER['REQUEST_METHOD'] === 'POST') {
            $nextPath = $controller->login($_POST);
            header("Location: $nextPath");
            exit;
        } else {
            $view = $controller->showLogin();
            require $view;
        }
    } elseif ($path === '/profile') {
        $controller = new ProfileController();
        $result = $controller->showProfile();
        if (is_array($result)) {
            extract($result['data']);
            require $result['view'];
        } else {
            require $result;
        }
    } elseif ($path === '/profile/update' && $_SERVER['REQUEST_METHOD'] === 'POST') {
        $controller = new ProfileController();
        $nextPath = $controller->updateProfile($_POST);
        header("Location: $nextPath");
        exit;
    } else {
        throw new Exception("Page non trouvée.");
    }
} catch (Exception $e) {
    $error = $e->getMessage();
    require '../views/home.php';
}
```

---

### **Explications**
- **PHP POO avec architecture MVC** : Le code utilise des classes (POO) pour le modèle (`User`), les contrôleurs (`AuthController`, `ProfileController`), et sépare la logique en modèle, vue, contrôleur (MVC).
- **Gestion des erreurs** : Les exceptions sont propagées jusqu’à `index.php`, qui les affiche sur `home.php`.
- **API BAN** : L’URL `https://api-adresse.data.gouv.fr/search/` est utilisée conformément à la documentation officielle.

---

### **Sécurité**
1. **Injection SQL** : Requêtes préparées avec PDO.
2. **XSS** : Échappement avec `htmlspecialchars()`.
3. **CSRF** : Token généré avec `random_bytes()` et validé.
4. **Mot de passe** : Hashé avec `password_hash()`, vérifié avec regex.

---

### **Test**
1. **Inscription** :
   - Valide : "Motdepasse123!" → Redirection vers `/login`.
   - Invalide : "Motdepasse!" → Erreur sur `/`.
2. **Connexion** :
   - Valide : Identifiants corrects → Redirection vers `/profile`.
   - Invalide : Mauvais mot de passe → Erreur sur `/`.
3. **Profil** :
   - Mise à jour valide → Reste sur `/profile`.
   - Email déjà pris → Erreur sur `/`.

