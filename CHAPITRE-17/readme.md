### **Chapitre 17 : PHP orienté objet (POO)**

#### **Introduction**
La programmation orientée objet (POO) est un paradigme qui permet de structurer un programme en regroupant des données et des comportements dans des entités appelées **objets**. En PHP, la POO est largement utilisée pour créer des applications modulaires, réutilisables et maintenables. Ce chapitre explore les bases de la POO, ses principes fondamentaux, son application dans des projets PHP, les design patterns courants, et une intégration pratique avec une API gouvernementale comme la **Base Adresse Nationale (BAN)**.

---

### **1. Concepts de base de la POO**

#### **1.1 Les classes et les objets**
- Une **classe** est un modèle ou un plan qui définit les propriétés (attributs) et les comportements (méthodes) d’un objet.
- Un **objet** est une instance d’une classe.

**Exemple :**
```php
class Voiture {
    public $marque; // Propriété
    public $couleur;

    // Constructeur
    public function __construct($marque, $couleur) {
        $this->marque = $marque;
        $this->couleur = $couleur;
    }

    // Méthode
    public function demarrer() {
        return "La voiture $this->marque démarre !";
    }
}

// Création d'un objet
$maVoiture = new Voiture("Toyota", "Rouge");
echo $maVoiture->demarrer(); // Affiche : La voiture Toyota démarre !
```

- `$this` fait référence à l’instance actuelle de la classe.
- Le constructeur `__construct` initialise les propriétés lors de la création de l’objet.

#### **1.2 Héritage**
L’héritage permet à une classe (classe enfant) d’hériter des propriétés et méthodes d’une autre classe (classe parent).

**Exemple :**
```php
class Vehicule {
    protected $type = "Véhicule";

    public function getType() {
        return $this->type;
    }
}

class Moto extends Vehicule {
    protected $type = "Moto";
}

// Utilisation
$maMoto = new Moto();
echo $maMoto->getType(); // Affiche : Moto
```

- Le mot-clé `extends` établit l’héritage.
- Une classe enfant peut redéfinir (override) ou compléter les méthodes du parent.

---

### **2. Principes fondamentaux de la POO**

#### **2.1 Encapsulation**
L’encapsulation consiste à masquer les détails internes d’une classe et à contrôler l’accès aux propriétés via des **modificateurs d’accès** :
- `public` : Accessible partout.
- `protected` : Accessible dans la classe et ses classes enfants.
- `private` : Accessible uniquement dans la classe elle-même.

**Exemple :**
```php
class CompteBancaire {
    private $solde;

    public function __construct($soldeInitial) {
        $this->solde = $soldeInitial;
    }

    public function deposer($montant) {
        if ($montant > 0) {
            $this->solde += $montant;
        }
    }

    public function getSolde() {
        return $this->solde;
    }
}

$monCompte = new CompteBancaire(100);
$monCompte->deposer(50);
echo $monCompte->getSolde(); // Affiche : 150
// $monCompte->solde; // Erreur : accès privé
```

#### **2.2 Abstraction**
L’abstraction consiste à définir des interfaces ou des classes abstraites pour spécifier *quoi* faire sans préciser *comment*.

**Exemple avec une classe abstraite :**
```php
abstract class Animal {
    abstract public function crier();

    public function seDeplacer() {
        return "L'animal se déplace.";
    }
}

class Chien extends Animal {
    public function crier() {
        return "Wouf !";
    }
}

$monChien = new Chien();
echo $monChien->crier(); // Affiche : Wouf !
```

- Une classe abstraite ne peut pas être instanciée directement.
- Les méthodes abstraites doivent être implémentées par les classes enfants.

#### **2.3 Polymorphisme**
Le polymorphisme permet à des objets de classes différentes de répondre différemment à la même méthode.

**Exemple :**
```php
interface Forme {
    public function calculerAire();
}

class Cercle implements Forme {
    private $rayon;

    public function __construct($rayon) {
        $this->rayon = $rayon;
    }

    public function calculerAire() {
        return pi() * $this->rayon * $this->rayon;
    }
}

class Rectangle implements Forme {
    private $longueur;
    private $largeur;

    public function __construct($longueur, $largeur) {
        $this->longueur = $longueur;
        $this->largeur = $largeur;
    }

    public function calculerAire() {
        return $this->longueur * $this->largeur;
    }
}

$formes = [new Cercle(5), new Rectangle(4, 6)];
foreach ($formes as $forme) {
    echo $forme->calculerAire() . "\n"; // Affiche l’aire pour chaque forme
}
```

- Les interfaces définissent un contrat que les classes doivent respecter.
- Le polymorphisme permet de traiter les objets de manière uniforme.

---

### **3. Utilisation de la POO pour structurer des projets PHP**

Dans un projet PHP, la POO permet de :
- Séparer les responsabilités (modèle MVC : Modèle-Vue-Contrôleur).
- Réutiliser le code grâce à l’héritage et aux interfaces.
- Faciliter la maintenance avec des classes bien encapsulées.

**Exemple d’architecture MVC simplifiée :**
```php
// Modèle
class Utilisateur {
    private $nom;

    public function __construct($nom) {
        $this->nom = $nom;
    }

    public function getNom() {
        return $this->nom;
    }
}

// Contrôleur
class UtilisateurController {
    private $utilisateur;

    public function __construct(Utilisateur $utilisateur) {
        $this->utilisateur = $utilisateur;
    }

    public function afficherNom() {
        return $this->utilisateur->getNom();
    }
}

// Utilisation
$utilisateur = new Utilisateur("Alice");
$controller = new UtilisateurController($utilisateur);
echo $controller->afficherNom(); // Affiche : Alice
```

- Le modèle représente les données.
- Le contrôleur gère la logique métier.
- La vue (non montrée ici) affiche les résultats.

---

### **4. Design patterns courants en PHP**

Les **design patterns** sont des solutions éprouvées à des problèmes récurrents. Voici deux exemples courants en PHP :

#### **4.1 Singleton**
Garantit qu’une classe n’a qu’une seule instance.

**Exemple :**
```php
class Database {
    private static $instance = null;

    private function __construct() {
        // Connexion à la base de données
    }

    public static function getInstance() {
        if (self::$instance === null) {
            self::$instance = new self();
        }
        return self::$instance;
    }
}

$db1 = Database::getInstance();
$db2 = Database::getInstance();
var_dump($db1 === $db2); // true : même instance
```

#### **4.2 Factory**
Crée des objets sans expliciter leur classe exacte.

**Exemple :**
```php
interface Transport {
    public function livrer();
}

class Camion implements Transport {
    public function livrer() {
        return "Livraison par camion.";
    }
}

class Bateau implements Transport {
    public function livrer() {
        return "Livraison par bateau.";
    }
}

class TransportFactory {
    public static function creerTransport($type) {
        switch ($type) {
            case 'camion':
                return new Camion();
            case 'bateau':
                return new Bateau();
            default:
                throw new Exception("Type de transport inconnu.");
        }
    }
}

$transport = TransportFactory::creerTransport("camion");
echo $transport->livrer(); // Affiche : Livraison par camion.
```

---

### **5. Intégration d’une API gouvernementale : Base Adresse Nationale (BAN)**

La **BAN** est une API publique française qui fournit des données d’adresses géolocalisées. Voici comment l’intégrer en POO avec PHP.

#### **Exemple :**
```php
class AdresseService {
    private $url = "https://api-adresse.data.gouv.fr/search/";

    public function rechercherAdresse($query) {
        $url = $this->url . "?q=" . urlencode($query);
        $response = file_get_contents($url);
        return json_decode($response, true);
    }

    public function formaterResultat($data) {
        if (isset($data['features'][0])) {
            $adresse = $data['features'][0]['properties']['label'];
            $coords = $data['features'][0]['geometry']['coordinates'];
            return "Adresse : $adresse, Coordonnées : " . implode(", ", $coords);
        }
        return "Aucune adresse trouvée.";
    }
}

// Utilisation
$service = new AdresseService();
$resultat = $service->rechercherAdresse("10 rue de la Paix, Paris");
echo $service->formaterResultat($resultat);
```

- La classe `AdresseService` encapsule la logique d’appel à l’API.
- `file_get_contents` est utilisé ici pour simplifier, mais en production, on préférerait `cURL` ou une bibliothèque comme Guzzle pour gérer les requêtes HTTP.

---

### **Conclusion**
La POO en PHP est un outil puissant pour structurer des projets complexes. Les concepts comme l’héritage, l’encapsulation, l’abstraction et le polymorphisme permettent de créer du code robuste et réutilisable. Les design patterns facilitent la résolution de problèmes courants, tandis que l’intégration d’APIs comme la BAN montre comment appliquer la POO à des cas réels. Avec ces bases, vous pouvez concevoir des applications PHP modernes et évolutives.

**Exercice pratique :**
1. Créez une classe `Personne` avec des propriétés privées et des méthodes publiques.
2. Implémentez une classe `Etudiant` qui hérite de `Personne`.
3. Ajoutez une méthode pour appeler l’API BAN avec une adresse saisie par l’utilisateur.
