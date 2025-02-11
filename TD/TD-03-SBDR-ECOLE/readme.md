
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
    email VARCHAR(100) UNIQUE,
    adresse VARCHAR(255)
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
    date_inscription DATE DEFAULT CURRENT_DATE,
    FOREIGN KEY (etudiant_id) REFERENCES etudiants(id) ON DELETE CASCADE,
    FOREIGN KEY (cours_id) REFERENCES cours(id) ON DELETE CASCADE
);
```

### b) Modification d'une table
```sql
ALTER TABLE etudiants ADD COLUMN telephone VARCHAR(15);
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
-- Insertion des étudiants
INSERT INTO etudiants (nom, prenom, age, email, adresse) VALUES 
('Dupont', 'Jean', 22, 'jean.dupont@email.com', '123 Rue de Paris'),
('Martin', 'Alice', 20, 'alice.martin@email.com', '456 Avenue des Champs'),
('Bernard', 'Luc', 21, 'luc.bernard@email.com', '789 Boulevard de Lyon');

-- Insertion des cours
INSERT INTO cours (titre, description) VALUES 
('Base de données', 'Introduction aux bases de données relationnelles.'),
('Programmation Python', 'Apprendre les bases de la programmation en Python.'),
('Réseaux informatiques', 'Comprendre les principes des réseaux informatiques.');

-- Insertion des inscriptions
INSERT INTO inscriptions (etudiant_id, cours_id, date_inscription) VALUES 
(1, 1, '2023-10-01'),
(1, 2, '2023-10-02'),
(2, 1, '2023-10-03'),
(3, 3, '2023-10-04');
```

### b) Modification de données existantes
```sql
UPDATE etudiants SET age = 23 WHERE id = 1;
```

### c) Suppression de données
```sql
DELETE FROM etudiants WHERE id = 3;
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

### c) Requêtes complexes avec jointures
```sql
-- Liste des étudiants inscrits à un cours spécifique
SELECT etudiants.nom, etudiants.prenom, cours.titre
FROM etudiants
INNER JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
INNER JOIN cours ON inscriptions.cours_id = cours.id
WHERE cours.titre = 'Base de données';

-- Liste des cours suivis par un étudiant spécifique
SELECT cours.titre, cours.description
FROM cours
INNER JOIN inscriptions ON cours.id = inscriptions.cours_id
INNER JOIN etudiants ON inscriptions.etudiant_id = etudiants.id
WHERE etudiants.nom = 'Dupont' AND etudiants.prenom = 'Jean';
```

---

## 4. Data Control Language (DCL) - Langage de Contrôle des Données
Utilisé pour accorder ou révoquer des permissions.

### a) Attribution des droits d'accès
```sql
GRANT SELECT, INSERT, UPDATE ON gestion_ecole.* TO 'utilisateur'@'localhost';
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

#### RIGHT JOIN : Renvoie tous les enregistrements de la table de droite et ceux correspondants de la table de gauche
```sql
SELECT etudiants.nom, cours.titre
FROM etudiants
RIGHT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
RIGHT JOIN cours ON inscriptions.cours_id = cours.id;
```

### b) Indexation pour optimiser les requêtes
```sql
CREATE INDEX idx_email ON etudiants(email);
CREATE INDEX idx_titre ON cours(titre);
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

---

## 8. Exemples de Requêtes Avancées

### a) Nombre d'étudiants inscrits par cours
```sql
SELECT cours.titre, COUNT(inscriptions.etudiant_id) AS nombre_etudiants
FROM cours
LEFT JOIN inscriptions ON cours.id = inscriptions.cours_id
GROUP BY cours.titre;
```

### b) Moyenne d'âge des étudiants par cours
```sql
SELECT cours.titre, AVG(etudiants.age) AS moyenne_age
FROM cours
INNER JOIN inscriptions ON cours.id = inscriptions.cours_id
INNER JOIN etudiants ON inscriptions.etudiant_id = etudiants.id
GROUP BY cours.titre;
```

### c) Étudiants non inscrits à un cours
```sql
SELECT etudiants.nom, etudiants.prenom
FROM etudiants
LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
WHERE inscriptions.cours_id IS NULL;
```

---

## 9. Jeu de Données Étendu

### a) Insertion de données supplémentaires
```sql
-- Ajout de nouveaux étudiants
INSERT INTO etudiants (nom, prenom, age, email, adresse) VALUES 
('Leroy', 'Sophie', 19, 'sophie.leroy@email.com', '101 Rue de Marseille'),
('Moreau', 'Pierre', 22, 'pierre.moreau@email.com', '202 Avenue de Bordeaux');

-- Ajout de nouveaux cours
INSERT INTO cours (titre, description) VALUES 
('Algorithmique', 'Apprendre les bases de l\'algorithmique.'),
('Sécurité informatique', 'Introduction à la sécurité des systèmes informatiques.');

-- Ajout de nouvelles inscriptions
INSERT INTO inscriptions (etudiant_id, cours_id, date_inscription) VALUES 
(4, 1, '2023-10-05'),
(4, 3, '2023-10-06'),
(5, 2, '2023-10-07'),
(5, 4, '2023-10-08');
```

### b) Requêtes sur le jeu de données étendu
```sql
-- Liste des cours suivis par chaque étudiant
SELECT etudiants.nom, etudiants.prenom, GROUP_CONCAT(cours.titre SEPARATOR ', ') AS cours_inscrits
FROM etudiants
LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
LEFT JOIN cours ON inscriptions.cours_id = cours.id
GROUP BY etudiants.id;

-- Nombre total d'inscriptions par étudiant
SELECT etudiants.nom, etudiants.prenom, COUNT(inscriptions.id) AS nombre_inscriptions
FROM etudiants
LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
GROUP BY etudiants.id;
```

---
Voici le schéma régénéré et expliqué en détail pour correspondre à la structure de la base de données générée précédemment. J'ai également ajouté des précisions sur les relations et les cardinalités.

---

## Schéma de la Base de Données

```plaintext
+-----------------+          +-----------------+          +-----------------+
|   etudiants     |          |   inscriptions  |          |      cours      |
+-----------------+          +-----------------+          +-----------------+
| id (PK)         |<---1---->| etudiant_id (FK) |          | id (PK)         |
| nom             |          | cours_id (FK)    |<---N---->| titre           |
| prenom          |          | date_inscription |          | description     |
| age             |          +-----------------+          +-----------------+
| email           |
| adresse         |
| telephone       |
+-----------------+
```

---

## Explication Détaillée du Schéma

### **1. Tables et leurs Colonnes**

#### **Table `etudiants`**
- **`id` (PK)** : Clé primaire (Primary Key) qui identifie de manière unique chaque étudiant.
- **`nom`** : Nom de l'étudiant.
- **`prenom`** : Prénom de l'étudiant.
- **`age`** : Âge de l'étudiant, avec une contrainte `CHECK` pour s'assurer que l'âge est supérieur ou égal à 18.
- **`email`** : Adresse e-mail de l'étudiant, avec une contrainte `UNIQUE` pour garantir qu'aucun étudiant n'a la même adresse e-mail.
- **`adresse`** : Adresse physique de l'étudiant.
- **`telephone`** : Numéro de téléphone de l'étudiant.

#### **Table `cours`**
- **`id` (PK)** : Clé primaire qui identifie de manière unique chaque cours.
- **`titre`** : Titre du cours.
- **`description`** : Description détaillée du cours.

#### **Table `inscriptions`**
- **`id` (PK)** : Clé primaire qui identifie de manière unique chaque inscription.
- **`etudiant_id` (FK)** : Clé étrangère (Foreign Key) qui référence l'`id` de la table `etudiants`. Elle lie un étudiant à une inscription.
- **`cours_id` (FK)** : Clé étrangère qui référence l'`id` de la table `cours`. Elle lie un cours à une inscription.
- **`date_inscription`** : Date à laquelle l'étudiant s'est inscrit au cours. Par défaut, elle prend la date du jour (`CURRENT_DATE`).

---

### **2. Relations et Cardinalités**

#### **Relation entre `etudiants` et `inscriptions`**
- **Cardinalité : 1:N** (Un à plusieurs)
  - **`1` côté `etudiants`** : Un étudiant peut s'inscrire à **plusieurs cours**.
  - **`N` côté `inscriptions`** : Plusieurs inscriptions peuvent être associées à un seul étudiant.
  - **Explication** : Un étudiant peut s'inscrire à plusieurs cours, mais chaque inscription est liée à un seul étudiant.

#### **Relation entre `cours` et `inscriptions`**
- **Cardinalité : 1:N** (Un à plusieurs)
  - **`1` côté `cours`** : Un cours peut avoir **plusieurs étudiants** inscrits.
  - **`N` côté `inscriptions`** : Plusieurs inscriptions peuvent être associées à un seul cours.
  - **Explication** : Un cours peut être suivi par plusieurs étudiants, mais chaque inscription est liée à un seul cours.

---

### **3. Rôle de la Table `inscriptions`**
- La table `inscriptions` sert de **table de liaison** (ou table d'association) entre les tables `etudiants` et `cours`.
- Elle permet de gérer une relation **many-to-many** (plusieurs-à-plusieurs) entre les étudiants et les cours.
- Chaque enregistrement dans cette table représente une inscription d'un étudiant à un cours.

---

### **4. Contraintes et Intégrité des Données**
- **Clés primaires (PK)** : Garantissent que chaque enregistrement dans une table est unique.
- **Clés étrangères (FK)** : Assurent l'intégrité référentielle entre les tables. Par exemple :
  - Un `etudiant_id` dans `inscriptions` doit correspondre à un `id` existant dans `etudiants`.
  - Un `cours_id` dans `inscriptions` doit correspondre à un `id` existant dans `cours`.
- **Contraintes supplémentaires** :
  - `UNIQUE` sur `email` dans `etudiants` : Empêche deux étudiants d'avoir la même adresse e-mail.
  - `CHECK` sur `age` dans `etudiants` : Garantit que l'âge est supérieur ou égal à 18.

---

### **5. Exemple de Données**

#### Table `etudiants`
| id  | nom     | prenom | age | email                   | adresse               | telephone   |
|-----|---------|--------|-----|-------------------------|-----------------------|-------------|
| 1   | Dupont  | Jean   | 22  | jean.dupont@email.com   | 123 Rue de Paris      | 0123456789  |
| 2   | Martin  | Alice  | 20  | alice.martin@email.com  | 456 Avenue des Champs | 0987654321  |

#### Table `cours`
| id  | titre                  | description                                      |
|-----|------------------------|--------------------------------------------------|
| 1   | Base de données        | Introduction aux bases de données relationnelles.|
| 2   | Programmation Python   | Apprendre les bases de la programmation en Python.|

#### Table `inscriptions`
| id  | etudiant_id | cours_id | date_inscription |
|-----|-------------|----------|-------------------|
| 1   | 1           | 1        | 2023-10-01        |
| 2   | 1           | 2        | 2023-10-02        |
| 3   | 2           | 1        | 2023-10-03        |

---

### **6. Requêtes pour Illustrer les Relations**

#### Liste des étudiants inscrits à un cours spécifique
```sql
SELECT etudiants.nom, etudiants.prenom, cours.titre
FROM etudiants
INNER JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
INNER JOIN cours ON inscriptions.cours_id = cours.id
WHERE cours.titre = 'Base de données';
```

#### Liste des cours suivis par un étudiant spécifique
```sql
SELECT cours.titre, cours.description
FROM cours
INNER JOIN inscriptions ON cours.id = inscriptions.cours_id
INNER JOIN etudiants ON inscriptions.etudiant_id = etudiants.id
WHERE etudiants.nom = 'Dupont' AND etudiants.prenom = 'Jean';
```

---
