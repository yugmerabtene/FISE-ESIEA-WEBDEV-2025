### TD : Système de Gestion d'Utilisateurs avec Base de Données MySQL et Sécurité

---

#### **Énoncé**

L'objectif de ce TD est de créer un système de gestion d'utilisateurs en **PHP procédural** avec une base de données **MySQL**. Le système permettra aux utilisateurs de :
1. Créer un compte.
2. Se connecter.
3. Accéder à une page de profil protégée.

Le frontend sera en **HTML/CSS**, et le backend en **PHP procédural**. Des mesures de sécurité contre les attaques **XSS** et **SQL Injection** seront mises en place.

---

#### **Fonctionnalités attendues :**

1. **Page d'accueil (`index.php`)** :
   - Une petite présentation dans le `main`.
   - Un menu avec des liens vers la page de connexion (`login.php`) et la page d'inscription (`register.php`).

2. **Page d'inscription (`register.php`)** :
   - Formulaire pour créer un compte (nom d'utilisateur et mot de passe).
   - Le mot de passe doit être **hashé** avant d'être stocké dans la base de données.
   - Vérification de l'unicité du nom d'utilisateur.
   - Redirection vers la page de connexion après inscription.

3. **Page de connexion (`login.php`)** :
   - Formulaire de connexion.
   - Redirection vers la page de profil après connexion.

4. **Page de profil (`profile.php`)** :
   - Page protégée accessible uniquement après connexion.
   - Affichage des informations de l'utilisateur (nom d'utilisateur) depuis la base de données.

5. **Déconnexion** :
   - Un lien pour se déconnecter et revenir à la page de connexion.

---

#### **Architecture du projet :**

```
/projet
    /assets
        /css
            style.css
        /img
    /functions
        getPDO.php
        createUser.php
        checkUser.php
        getUserInfo.php
        logout.php
    index.php
    login.php
    register.php
    profile.php
    header.php
    footer.php
```

---

### **Étapes et Code Complet**

---

#### **1. Création de la Base de Données MySQL**

Exécutez le script SQL suivant pour créer la base de données et la table `users` :

```sql
CREATE DATABASE user_management;

USE user_management;

CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL
);
```

---

#### **2. Fichier `header.php`**

Ce fichier contient l'en-tête commun à toutes les pages.

```php
<?php
// Démarre la session pour gérer les utilisateurs connectés
session_start();
?>
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>User Management System</title>
    <link rel="stylesheet" href="assets/css/style.css">
</head>
<body>
    <header>
        <h1>User Management System</h1>
        <nav>
            <?php if (isset($_SESSION['username'])) : ?>
                <!-- Si l'utilisateur est connecté, affiche les liens vers le profil et la déconnexion -->
                <a href="profile.php">Profile</a> |
                <a href="functions/logout.php">Logout</a>
            <?php else : ?>
                <!-- Si l'utilisateur n'est pas connecté, affiche les liens vers la page d'accueil, de connexion et d'inscription -->
                <a href="index.php">Home</a> |
                <a href="login.php">Login</a> |
                <a href="register.php">Register</a>
            <?php endif; ?>
        </nav>
    </header>
```

---

#### **3. Fichier `footer.php`**

Ce fichier contient le pied de page commun à toutes les pages.

```php
    <footer>
        <p>&copy; 2023 - User Management System</p>
    </footer>
</body>
</html>
```

---

#### **4. Fichier `index.php`**

Ce fichier contient une petite présentation et un menu avec des liens vers les pages de connexion et d'inscription.

```php
<?php
// Inclut l'en-tête de la page
require 'header.php';
?>

<main>
    <h2>Welcome to the User Management System</h2>
    <p>This is a simple user management system where you can register, login, and view your profile.</p>
    <p>Please use the menu above to navigate to the login or registration page.</p>
</main>

<?php
// Inclut le pied de page de la page
require 'footer.php';
?>
```

---

#### **5. Fichier `login.php`**

Ce fichier gère la connexion des utilisateurs.

```php
<?php
// Inclut l'en-tête de la page
require 'header.php';

// Inclut les fonctions nécessaires
require 'functions/getPDO.php';
require 'functions/login.php';

// Gestion de la connexion
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Récupère et sécurise les données du formulaire
    $username = htmlspecialchars($_POST['username'], ENT_QUOTES, 'UTF-8');
    $password = $_POST['password'];

    // Vérifie les informations de connexion
    if (login($username, $password)) {
        // Si la connexion est réussie, enregistre le nom d'utilisateur en session et redirige vers le profil
        $_SESSION['username'] = $username;
        header('Location: profile.php');
        exit;
    } else {
        // Si la connexion échoue, affiche un message d'erreur
        echo "<p>Invalid username or password.</p>";
    }
}
?>
<main>
    <h2>Login</h2>
    <form method="POST" action="login.php">
        <label for="username">Username :</label>
        <input type="text" id="username" name="username" required>
        <br>
        <label for="password">Password :</label>
        <input type="password" id="password" name="password" required>
        <br>
        <button type="submit">Login</button>
    </form>
    <p>Don't have an account? <a href="register.php">Register here</a>.</p>
</main>

<?php
// Inclut le pied de page de la page
require 'footer.php';
?>
```

---

#### **6. Fichier `register.php`**

Ce fichier gère l'inscription des utilisateurs.

```php
<?php
// Inclut l'en-tête de la page
require 'header.php';

// Inclut les fonctions nécessaires
require 'functions/getPDO.php';
require 'functions/createUser.php';

// Gestion de l'inscription
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Récupère et sécurise les données du formulaire
    $username = htmlspecialchars($_POST['username'], ENT_QUOTES, 'UTF-8');
    $password = $_POST['password'];

    // Vérifie si le nom d'utilisateur est déjà utilisé
    $pdo = getPDO();
    $sql = "SELECT username FROM users WHERE username = :username";
    $stmt = $pdo->prepare($sql);
    $stmt->execute(['username' => $username]);
    $existingUser = $stmt->fetch(PDO::FETCH_ASSOC);

    if ($existingUser) {
        // Si le nom d'utilisateur existe déjà, affiche un message d'erreur
        echo "<p>Username already exists. Please choose a different username.</p>";
    } else {
        // Crée un nouvel utilisateur
        if (createUser($username, $password)) {
            // Si l'inscription est réussie, redirige vers la page de connexion avec un message de succès
            $_SESSION['success_message'] = "Registration successful! Please login.";
            header('Location: login.php');
            exit;
        } else {
            // Si l'inscription échoue, affiche un message d'erreur
            echo "<p>Error during account creation.</p>";
        }
    }
}
?>
<main>
    <h2>Register</h2>
    <form method="POST" action="register.php">
        <label for="username">Username :</label>
        <input type="text" id="username" name="username" required>
        <br>
        <label for="password">Password :</label>
        <input type="password" id="password" name="password" required>
        <br>
        <button type="submit">Register</button>
    </form>
    <p>Already have an account? <a href="login.php">Login here</a>.</p>
</main>

<?php
// Inclut le pied de page de la page
require 'footer.php';
?>
```

---

#### **7. Fichier `profile.php`**

Ce fichier affiche la page de profil protégée avec les informations de l'utilisateur depuis la base de données.

```php
<?php
// Inclut l'en-tête de la page
require 'header.php';

// Vérifie si l'utilisateur est connecté
if (!isset($_SESSION['username'])) {
    // Si l'utilisateur n'est pas connecté, redirige vers la page de connexion
    header('Location: login.php');
    exit;
}

// Inclut les fonctions nécessaires
require 'functions/getUserInfo.php';

// Récupère les informations de l'utilisateur depuis la base de données
$user = getUserInfo($_SESSION['username']);

if ($user) {
    // Affiche les informations de l'utilisateur
    echo "<h2>Welcome, " . htmlspecialchars($user['username'], ENT_QUOTES, 'UTF-8') . " !</h2>";
    echo "<p>Here are your details :</p>";
    echo "<ul>";
    echo "<li><strong>Username :</strong> " . htmlspecialchars($user['username'], ENT_QUOTES, 'UTF-8') . "</li>";
    echo "</ul>";
} else {
    // Si une erreur survient lors de la récupération des informations, affiche un message d'erreur
    echo "<p>Error retrieving user information.</p>";
}

// Inclut le pied de page de la page
require 'footer.php';
?>
```

---

#### **8. Fichier `functions/getPDO.php`**

Ce fichier contient la fonction pour établir une connexion à la base de données.

```php
<?php
function getPDO() {
    // Informations de connexion à la base de données
    $servername = "localhost";
    $username = "root";
    $password = "";
    $dbname = "user_management";

    try {
        // Crée une nouvelle instance de PDO pour se connecter à la base de données
        $pdo = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
        // Configure PDO pour générer des exceptions en cas d'erreur
        $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        return $pdo;
    } catch (PDOException $e) {
        // En cas d'erreur de connexion, affiche un message d'erreur générique et arrête le script
        die("Database connection error. Please try again later.");
    }
}
?>
```

---

#### **9. Fichier `functions/createUser.php`**

Ce fichier contient la fonction pour créer un nouvel utilisateur.

```php
<?php
// Inclut le fichier de connexion à la base de données
require 'getPDO.php';

function createUser($username, $password) {
    // Obtient une instance de PDO
    $pdo = getPDO();
    // Hash le mot de passe avant de le stocker dans la base de données
    $hashedPassword = password_hash($password, PASSWORD_BCRYPT);
    // Prépare la requête SQL pour insérer un nouvel utilisateur
    $sql = "INSERT INTO users (username, password) VALUES (:username, :password)";
    $stmt = $pdo->prepare($sql);
    // Exécute la requête avec les paramètres fournis
    return $stmt->execute(['username' => $username, 'password' => $hashedPassword]);
}
?>
```

---

#### **10. Fichier `functions/login.php`**

Ce fichier contient la fonction pour vérifier les informations de connexion.

```php
<?php
// Inclut le fichier de connexion à la base de données
require 'getPDO.php';

function login($username, $password) {
    // Obtient une instance de PDO
    $pdo = getPDO();
    // Prépare la requête SQL pour récupérer le mot de passe hashé de l'utilisateur
    $sql = "SELECT password FROM users WHERE username = :username";
    $stmt = $pdo->prepare($sql);
    // Exécute la requête avec le nom d'utilisateur fourni
    $stmt->execute(['username' => $username]);
    // Récupère le résultat de la requête
    $result = $stmt->fetch(PDO::FETCH_ASSOC);

    // Vérifie si l'utilisateur existe et si le mot de passe fourni correspond au mot de passe hashé
    if ($result && password_verify($password, $result['password'])) {
        return true;
    }
    return false;
}
?>
```

---

#### **11. Fichier `functions/getUserInfo.php`**

Ce fichier contient la fonction pour récupérer les informations d'un utilisateur.

```php
<?php
// Inclut le fichier de connexion à la base de données
require 'getPDO.php';

function getUserInfo($username) {
    // Obtient une instance de PDO
    $pdo = getPDO();
    // Prépare la requête SQL pour récupérer les informations de l'utilisateur
    $sql = "SELECT username, password FROM users WHERE username = :username";
    $stmt = $pdo->prepare($sql);
    // Exécute la requête avec le nom d'utilisateur fourni
    $stmt->execute(['username' => $username]);
    // Récupère et retourne les informations de l'utilisateur
    return $stmt->fetch(PDO::FETCH_ASSOC);
}
?>
```

---

#### **12. Fichier `functions/logout.php`**

Ce fichier gère la déconnexion.

```php
<?php
// Démarre la session
session_start();
// Détruit la session
session_destroy();
// Redirige vers la page de connexion
header('Location: login.php');
exit;
?>
```

---

#### **13. Fichier `assets/css/style.css`**

Ce fichier contient le style CSS pour le projet.

```css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

header {
    background-color: #333;
    color: #fff;
    padding: 10px 20px;
    text-align: center;
}

nav a {
    color: #fff;
    text-decoration: none;
    margin: 0 10px;
}

main {
    padding: 20px;
}

form {
    max-width: 300px;
    margin: 50px auto;
    padding: 20px;
    background: #fff;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

label {
    display: block;
    margin-bottom: 5px;
}

input[type="text"], input[type="password"] {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

button {
    width: 100%;
    padding: 10px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #555;
}

footer {
    text-align: center;
    padding: 20px;
    background-color: #333;
    color: #fff;
    position: fixed;
    bottom: 0;
    width: 100%;
}
```

---

### **Mesures de Sécurité**

1. **Contre les attaques XSS** :
   - Utilisation de `htmlspecialchars()` pour échapper les caractères spéciaux dans les entrées utilisateur avant de les afficher.

2. **Contre les attaques SQL Injection** :
   - Utilisation de requêtes préparées avec `PDO` pour éviter les injections SQL.

3. **Hachage des mots de passe** :
   - Utilisation de `password_hash()` pour hasher les mots de passe avant de les stocker dans la base de données.

4. **Vérification de l'unicité du nom d'utilisateur** :
   - Avant de créer un nouvel utilisateur, vérifie si le nom d'utilisateur existe déjà dans la base de données.

---

### **En Résumé ce que vous avez appris**

Ce TD vous a permis de créer un système de gestion d'utilisateurs complet avec une base de données MySQL et des mesures de sécurité contre les attaques XSS et SQL Injection. Vous avez appris à :
- Créer une base de données et une table MySQL.
- Hasher les mots de passe avant de les stocker.
- Utiliser PHP pour interagir avec la base de données de manière sécurisée.
- Gérer les sessions pour protéger les pages.
- Afficher les informations de l'utilisateur depuis la base de données