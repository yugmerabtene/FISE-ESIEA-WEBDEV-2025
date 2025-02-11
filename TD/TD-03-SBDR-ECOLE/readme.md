# Travaux Dirigés : Base de Données d'une École Informatique

## 1. Data Definition Language (DDL) - Langage de Définition de Données
Utilisé pour créer et modifier la structure des bases de données.

### a) Création de la base de données et des tables
```sql
CREATE DATABASE gestion_ecole;
USE gestion_ecole;

CREATE TABLE etudiants (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18),
    email VARCHAR(100) UNIQUE
);

CREATE TABLE cours (
    id INT AUTO_INCREMENT PRIMARY KEY,
    titre VARCHAR(100) NOT NULL,
    description TEXT
);

CREATE TABLE inscriptions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    etudiant_id INT,
    cours_id INT,
    FOREIGN KEY (etudiant_id) REFERENCES etudiants(id),
    FOREIGN KEY (cours_id) REFERENCES cours(id)
);
```

### b) Modification d'une table
```sql
ALTER TABLE etudiants ADD COLUMN adresse VARCHAR(255);
```

### c) Suppression d'une table
```sql
DROP TABLE etudiants;
```

### d) Suppression de toutes les lignes sans supprimer la structure
```sql
TRUNCATE TABLE etudiants;
```

---

## 2. Data Manipulation Language (DML) - Langage de Manipulation des Données
Utilisé pour insérer, modifier et supprimer des données.

### a) Insertion de données
```sql
INSERT INTO etudiants (nom, prenom, age, email) VALUES ('Dupont', 'Jean', 22, 'jean.dupont@email.com');
INSERT INTO cours (titre, description) VALUES ('Base de données', 'Introduction aux bases de données relationnelles.');
INSERT INTO inscriptions (etudiant_id, cours_id) VALUES (1, 1);
```

### b) Modification de données existantes
```sql
UPDATE etudiants SET age = 23 WHERE id = 1;
```

### c) Suppression de données
```sql
DELETE FROM etudiants WHERE id = 1;
```

---

## 3. Data Query Language (DQL) - Langage de Requête des Données
Utilisé pour interroger la base de données.

### a) Extraction de toutes les données
```sql
SELECT * FROM etudiants;
```

### b) Extraction avec condition
```sql
SELECT nom, prenom FROM etudiants WHERE age > 20;
```

---

## 4. Data Control Language (DCL) - Langage de Contrôle des Données
Utilisé pour accorder ou révoquer des permissions.

### a) Attribution des droits d'accès
```sql
GRANT SELECT, INSERT ON gestion_ecole.* TO 'utilisateur'@'localhost';
```

### b) Retrait des droits d'accès
```sql
REVOKE INSERT ON gestion_ecole.* FROM 'utilisateur'@'localhost';
```

---

## 5. Transaction Control Language (TCL) - Langage de Contrôle des Transactions
Utilisé pour gérer les transactions.

### a) Validation des modifications
```sql
COMMIT;
```

### b) Annulation des modifications en cours
```sql
ROLLBACK;
```

### c) Création d'un point de sauvegarde
```sql
SAVEPOINT point1;
```

---

## 6. Jointures et Optimisation des Requêtes

### a) Types de jointures

#### INNER JOIN : Renvoie les enregistrements correspondants des deux tables
```sql
SELECT etudiants.nom, cours.titre
FROM etudiants
INNER JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
INNER JOIN cours ON inscriptions.cours_id = cours.id;
```

#### LEFT JOIN : Renvoie tous les enregistrements de la table de gauche et ceux correspondants de la table de droite
```sql
SELECT etudiants.nom, cours.titre
FROM etudiants
LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
LEFT JOIN cours ON inscriptions.cours_id = cours.id;
```

### b) Indexation pour optimiser les requêtes
```sql
CREATE INDEX idx_email ON etudiants(email);
```

### c) Bonnes pratiques d'optimisation
- Éviter `SELECT *`, préférer sélectionner uniquement les colonnes nécessaires.
- Utiliser des index sur les colonnes fréquemment utilisées dans `WHERE` ou `JOIN`.
- Supprimer les index inutiles pour réduire la charge sur les insertions/modifications.
- Utiliser des vues (`VIEW`) pour éviter de recalculer les résultats souvent sollicités.

---

## 7. Modèle Physique des Données (MPD)

### Explication des relations
- La table `etudiants` contient les informations des élèves.
- La table `cours` contient les informations des cours dispensés.
- La table `inscriptions` sert de table d'association entre `etudiants` et `cours` (relation many-to-many).
- Chaque inscription lie un `etudiant_id` à un `cours_id`.
- Des clés étrangères assurent l'intégrité des données entre ces entités.


