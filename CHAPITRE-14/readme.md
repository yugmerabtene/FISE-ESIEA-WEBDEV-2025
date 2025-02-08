# Chapitre 14 : PHP Procédural

## Introduction
PHP est un langage de programmation côté serveur utilisé principalement pour le développement web. Il permet de générer du contenu dynamique, traiter des formulaires, interagir avec des bases de données et gérer des sessions utilisateur.

---

## Syntaxe de base de PHP

Un script PHP commence par `<?php` et se termine par `?>`. PHP s'intègre directement dans un fichier HTML.

### Exemples de base
```php
<?php
// Affichage d'un texte
echo "Hello, World!";
?>
```

### Commentaires
- **Commentaire sur une ligne** : `// Ceci est un commentaire`
- **Commentaire multi-lignes** : `/* Ceci est un commentaire multi-lignes */`

### Affichage en PHP
```php
<?php
echo "<h1>Bienvenue sur mon site</h1>";
?>
```

---

## Variables, Boucles, Conditions et Fonctions

### Variables et Types
PHP est un langage faiblement typé, ce qui signifie qu'il détermine automatiquement le type de variable.
```php
<?php
$nom = "Jean";   // Chaîne de caractères
$age = 25;        // Entier
$prix = 19.99;    // Float
$actif = true;    // Booléen
?>
```

### Conditions
PHP prend en charge `if`, `else`, `elseif` et `switch`.
```php
<?php
$age = 20;
if ($age >= 18) {
    echo "Majeur";
} elseif ($age == 17) {
    echo "Presque majeur";
} else {
    echo "Mineur";
}
?>
```

Utilisation de `switch` :
```php
<?php
$jour = "Lundi";
switch ($jour) {
    case "Lundi":
        echo "Début de semaine";
        break;
    case "Vendredi":
        echo "Fin de semaine";
        break;
    default:
        echo "Journée normale";
}
?>
```

### Boucles
#### **Boucle for**
```php
for ($i = 0; $i < 5; $i++) {
    echo "Nombre : $i <br>";
}
```

#### **Boucle while**
```php
$i = 0;
while ($i < 5) {
    echo "Nombre : $i <br>";
    $i++;
}
```

#### **Boucle foreach (pour les tableaux)**
```php
$fruits = ["Pomme", "Banane", "Orange"];
foreach ($fruits as $fruit) {
    echo "$fruit <br>";
}
```

### Fonctions
```php
function saluer($nom) {
    return "Bonjour, " . $nom;
}
echo saluer("Alice");
```

---

## Interaction avec les formulaires HTML

PHP permet de récupérer des données envoyées via un formulaire en utilisant `$_POST` ou `$_GET`.

**Exemple d'un formulaire HTML :**
```html
<form method="POST" action="traitement.php">
    Nom : <input type="text" name="nom">
    <input type="submit" value="Envoyer">
</form>
```

**Traitement des données en PHP (`traitement.php`)**
```php
<?php
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $nom = htmlspecialchars($_POST['nom']);
    echo "Bonjour, " . $nom;
}
?>
```

---

## Organisation du Code avec `include` et `namespace`

### Inclusion de fichiers
Plutôt que de répéter du code, on peut utiliser `include` ou `require` pour inclure des fichiers PHP externes.

**Exemple :**
```php
// fichier config.php
<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "ma_base";
?>
```

Dans le fichier principal :
```php
<?php
require 'config.php';
echo "Configuration chargée !";
?>
```

### Utilisation des Namespaces
Les **namespaces** permettent d'organiser le code et d'éviter les conflits entre noms de classes ou de fonctions.

**Exemple :**
```php
namespace MonProjet;
class Utilisateur {
    public function direBonjour() {
        return "Bonjour!";
    }
}
```

Utilisation du namespace :
```php
<?php
require 'Utilisateur.php';
use MonProjet\Utilisateur;
$u = new Utilisateur();
echo $u->direBonjour();
?>
```

---

## Connexion à une base de données MySQL avec requêtes préparées

### Connexion avec PDO et `try-catch`
```php
<?php
require 'config.php';

try {
    $conn = new PDO("mysql:host=$servername;dbname=$dbname", $username, $password);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    echo "Connexion réussie";
} catch (PDOException $e) {
    echo "Erreur de connexion : " . $e->getMessage();
}
?>
```

### Requêtes préparées pour éviter les injections SQL

- **Insertion de données**
```php
$sql = "INSERT INTO utilisateurs (nom, email) VALUES (:nom, :email)";
$stmt = $conn->prepare($sql);
$stmt->execute(['nom' => "Alice", 'email' => "alice@example.com"]);
```

- **Sélection sécurisée de données**
```php
$sql = "SELECT nom, email FROM utilisateurs WHERE id = :id";
$stmt = $conn->prepare($sql);
$stmt->execute(['id' => 1]);
$result = $stmt->fetchAll(PDO::FETCH_ASSOC);
foreach ($result as $row) {
    echo "Nom : " . $row["nom"] . " - Email : " . $row["email"] . "<br>";
}
```