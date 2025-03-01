### **Cahier des charges**

#### **Objectif**
Développer un système de gestion d’utilisateurs permettant :
- L’inscription avec nom, prénom, email (unique), adresse (via API BAN), mot de passe (saisi deux fois, min. 12 caractères avec majuscule, minuscule, caractère spécial, chiffre).
- La connexion sécurisée via email et mot de passe, redirigeant vers une page de profil.
- La modification des informations personnelles dans le profil.

#### **Fonctionnalités principales**
1. **Inscription** :
   - Formulaire avec nom, prénom, email (unique), adresse (autocomplétée via API BAN), mot de passe (vérifié avec regex), et confirmation.
2. **Connexion** :
   - Authentification via email et mot de passe avec redirection vers `/profile`.
3. **Profil** :
   - Affichage et modification des informations personnelles (nom, prénom, email, adresse).
4. **Sécurité** :
   - Protection contre CSRF (token), XSS (échappement), injection SQL (requêtes préparées).
5. **Base de données** :
   - Table `users` : id, nom, prénom, email (unique), adresse, mot_de_passe (hashé).

#### **Technologies**
- **Backend** : PHP (MVC), MySQL.
- **Frontend** : HTML, CSS, JavaScript (fetch pour l’API BAN).
- **API externe** : BAN ([https://api-adresse.data.gouv.f](https://api.gouv.fr/les-api/base-adresse-nationale)r).

---

### **User Stories**

1. **En tant qu’utilisateur non inscrit**, je veux pouvoir m’inscrire avec mon nom, prénom, email, adresse et mot de passe afin de créer un compte.
   - **Critères d’acceptation** :
     - Formulaire avec nom, prénom, email (unique), adresse (autocomplétée), mot de passe (min. 12 caractères, majuscule, minuscule, caractère spécial, chiffre), et confirmation.
     - Erreur si l’email existe déjà ou si le mot de passe ne respecte pas les critères.

2. **En tant qu’utilisateur inscrit**, je veux me connecter avec mon email et mot de passe pour accéder à mon profil.
   - **Critères d’acceptation** :
     - Connexion réussie redirige vers `/profile`.
     - Erreur si email ou mot de passe incorrect.

3. **En tant qu’utilisateur connecté**, je veux voir et modifier mes informations personnelles sur ma page de profil.
   - **Critères d’acceptation** :
     - Champs nom, prénom, email, adresse pré-remplis et modifiables.
     - Email modifié doit rester unique.

4. **En tant qu’administrateur du système**, je veux que les données soient sécurisées contre les attaques CSRF, XSS et injections SQL.
   - **Critères d’acceptation** :
     - Token CSRF dans chaque formulaire.
     - Entrées échappées pour éviter XSS.
     - Requêtes SQL préparées.

---

### **Cas d’acceptation détaillés**

#### **Inscription**
- **Scénario 1 : Inscription réussie**
  - Given : Formulaire rempli (nom: "Jean", prénom: "Dupont", email: "jean.dupont@example.com", adresse: "10 rue de la Paix, Paris", mot de passe: "Motdepasse123!", confirmation: "Motdepasse123!").
  - When : Clic sur "S’inscrire".
  - Then : Compte créé, redirection vers `/login`.

- **Scénario 2 : Mot de passe invalide**
  - Given : Mot de passe "Motdepasse!" (pas de chiffre, < 12 caractères).
  - When : Soumission du formulaire.
  - Then : Erreur : "Le mot de passe doit contenir au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial."

- **Scénario 3 : Email déjà utilisé**
  - Given : Email "jean.dupont@example.com" existe.
  - When : Soumission avec cet email.
  - Then : Erreur : "Cet email est déjà utilisé."

#### **Connexion**
- **Scénario 1 : Connexion réussie**
  - Given : Identifiants corrects (email: "jean.dupont@example.com", mot de passe: "Motdepasse123!").
  - When : Clic sur "Se connecter".
  - Then : Redirection vers `/profile`.

#### **Modification profil**
- **Scénario 1 : Mise à jour réussie**
  - Given : Email modifié de "jean.dupont@example.com" à "marc.dupont@example.com".
  - When : Clic sur "Enregistrer".
  - Then : Données mises à jour si nouvel email unique.

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
│   └── profile.php
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
        $this->pdo = new PDO("mysql:host=localhost;dbname=gest_users", "root", "");
        $this->pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
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
        // Regex mise à jour : min 12 caractères, majuscule, minuscule, chiffre, caractère spécial
        if (!preg_match('/^(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[!@#$%^&*]).{12,}$/', $mot_de_passe)) {
            throw new Exception("Le mot de passe doit contenir au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial (ex. !@#$%^&*).");
        }
        if ($this->emailExists($email)) {
            throw new Exception("Cet email est déjà utilisé.");
        }
        $hash = password_hash($mot_de_passe, PASSWORD_BCRYPT);
        $stmt = $this->db->prepare("INSERT INTO users (nom, prenom, email, adresse, mot_de_passe) VALUES (?, ?, ?, ?, ?)");
        return $stmt->execute([$nom, $prenom, $email, $adresse, $hash]);
    }

    public function login($email, $mot_de_passe) {
        $stmt = $this->db->prepare("SELECT * FROM users WHERE email = ?");
        $stmt->execute([$email]);
        $user = $stmt->fetch(PDO::FETCH_ASSOC);
        if ($user && password_verify($mot_de_passe, $user['mot_de_passe'])) {
            return $user;
        }
        return false;
    }

    public function update($id, $nom, $prenom, $email, $adresse) {
        $stmt = $this->db->prepare("SELECT id FROM users WHERE email = ? AND id != ?");
        $stmt->execute([$email, $id]);
        if ($stmt->fetch()) {
            throw new Exception("Cet email est déjà utilisé par un autre utilisateur.");
        }
        $stmt = $this->db->prepare("UPDATE users SET nom = ?, prenom = ?, email = ?, adresse = ? WHERE id = ?");
        return $stmt->execute([$nom, $prenom, $email, $adresse, $id]);
    }

    private function emailExists($email) {
        $stmt = $this->db->prepare("SELECT id FROM users WHERE email = ?");
        $stmt->execute([$email]);
        return $stmt->fetch() !== false;
    }
}
```

#### **Explication de la regex**
- `^` : Début de la chaîne.
- `(?=.*[A-Z])` : Au moins une majuscule.
- `(?=.*[a-z])` : Au moins une minuscule.
- `(?=.*[0-9])` : Au moins un chiffre.
- `(?=.*[!@#$%^&*])` : Au moins un caractère spécial parmi `!@#$%^&*`.
- `.{12,}` : Au moins 12 caractères.
- `$` : Fin de la chaîne.

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
        require '../views/register.php';
    }

    public function register($data) {
        if ($_POST['csrf_token'] !== $_SESSION['csrf_token']) {
            die("Erreur CSRF");
        }
        if ($data['mot_de_passe'] !== $data['mot_de_passe_confirm']) {
            $error = "Les mots de passe ne correspondent pas.";
            require '../views/register.php';
            return;
        }
        try {
            $this->user->register($data['nom'], $data['prenom'], $data['email'], $data['adresse'], $data['mot_de_passe']);
            header("Location: /login");
        } catch (Exception $e) {
            $error = $e->getMessage();
            require '../views/register.php';
        }
    }

    public function showLogin() {
        require '../views/login.php';
    }

    public function login($data) {
        $user = $this->user->login($data['email'], $data['mot_de_passe']);
        if ($user) {
            $_SESSION['user_id'] = $user['id'];
            header("Location: /profile");
        } else {
            $error = "Email ou mot de passe incorrect.";
            require '../views/login.php';
        }
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
            header("Location: /login");
            exit;
        }
        $stmt = $this->user->db->prepare("SELECT * FROM users WHERE id = ?");
        $stmt->execute([$_SESSION['user_id']]);
        $user = $stmt->fetch(PDO::FETCH_ASSOC);
        require '../views/profile.php';
    }

    public function updateProfile($data) {
        if (!isset($_SESSION['user_id'])) {
            header("Location: /login");
            exit;
        }
        try {
            $this->user->update($_SESSION['user_id'], $data['nom'], $data['prenom'], $data['email'], $data['adresse']);
            header("Location: /profile");
        } catch (Exception $e) {
            $error = $e->getMessage();
            $stmt = $this->user->db->prepare("SELECT * FROM users WHERE id = ?");
            $stmt->execute([$_SESSION['user_id']]);
            $user = $stmt->fetch(PDO::FETCH_ASSOC);
            require '../views/profile.php';
        }
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
    <?php if (isset($error)): ?>
        <p style="color: red;"><?php echo htmlspecialchars($error); ?></p>
    <?php endif; ?>
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
    <?php if (isset($error)): ?>
        <p style="color: red;"><?php echo htmlspecialchars($error); ?></p>
    <?php endif; ?>
    <form method="POST" action="/login">
        <label>Email : <input type="email" name="email" required></label><br>
        <label>Mot de passe : <input type="password" name="mot_de_passe" required></label><br>
        <button type="submit">Se connecter</button>
    </form>
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
    <?php if (isset($error)): ?>
        <p style="color: red;"><?php echo htmlspecialchars($error); ?></p>
    <?php endif; ?>
    <form method="POST" action="/profile/update">
        <label>Nom : <input type="text" name="nom" value="<?php echo htmlspecialchars($user['nom']); ?>" required></label><br>
        <label>Prénom : <input type="text" name="prenom" value="<?php echo htmlspecialchars($user['prenom']); ?>" required></label><br>
        <label>Email : <input type="email" name="email" value="<?php echo htmlspecialchars($user['email']); ?>" required></label><br>
        <label>Adresse : <input type="text" id="adresse" name="adresse" value="<?php echo htmlspecialchars($user['adresse']); ?>" required></label>
        <div id="adresse-suggestions"></div><br>
        <button type="submit">Enregistrer</button>
    </form>
    <script src="/public/script.js"></script>
</body>
</html>
```

#### **9. JavaScript (public/script.js)**
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

#### **10. CSS (public/style.css)**
```css
body { font-family: Arial, sans-serif; padding: 20px; }
form { max-width: 400px; margin: 0 auto; }
label { display: block; margin: 10px 0; }
input { width: 100%; padding: 8px; }
button { background: #007BFF; color: white; padding: 10px; border: none; cursor: pointer; }
#adresse-suggestions { border: 1px solid #ddd; max-height: 150px; overflow-y: auto; }
#adresse-suggestions div { padding: 5px; cursor: pointer; }
#adresse-suggestions div:hover { background: #f0f0f0; }
```

#### **11. Point d’entrée (public/index.php)**
```php
<?php
require_once '../controllers/AuthController.php';
require_once '../controllers/ProfileController.php';

$path = parse_url($_SERVER['REQUEST_URI'], PHP_URL_PATH);

if ($path === '/register') {
    $controller = new AuthController();
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $controller->register($_POST);
    } else {
        $controller->showRegister();
    }
} elseif ($path === '/login') {
    $controller = new AuthController();
    if ($_SERVER['REQUEST_METHOD'] === 'POST') {
        $controller->login($_POST);
    } else {
        $controller->showLogin();
    }
} elseif ($path === '/profile') {
    $controller = new ProfileController();
    $controller->showProfile();
} elseif ($path === '/profile/update' && $_SERVER['REQUEST_METHOD'] === 'POST') {
    $controller = new ProfileController();
    $controller->updateProfile($_POST);
} else {
    echo "Page non trouvée.";
}
```

---

### **Sécurité**
1. **Injection SQL** : Requêtes préparées avec PDO.
2. **XSS** : Échappement avec `htmlspecialchars()` dans les vues.
3. **CSRF** : Token généré avec `random_bytes()` et validé côté serveur.
4. **Mot de passe** : Hashé avec `password_hash()`, vérifié avec regex (12 caractères, majuscule, minuscule, chiffre, caractère spécial).

---

### **Bonnes pratiques**
- Structure MVC claire.
- Gestion des erreurs avec exceptions.
- Validation côté serveur et client.
- Code modulaire et documenté.

---

### **Test**
1. **Inscription** :
   - Valide : "Motdepasse123!" (12 caractères, majuscule, minuscule, chiffre, caractère spécial).
   - Invalide : "Motdepasse!" (pas de chiffre), "motdepasse123!" (pas de majuscule).
2. **Connexion** : Vérifiez la redirection vers `/profile`.
3. **Profil** : Modifiez un email et testez l’unicité.
