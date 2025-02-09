# Chapitre 13 : Bases de Données avec MySQL

## Introduction aux bases de données relationnelles

Une base de données relationnelle est un ensemble de données organisées en tables interconnectées. MySQL est un **Système de Gestion de Base de Données Relationnelle (SGBDR)** qui permet de stocker, manipuler et gérer ces données efficacement.

### Concepts clés
- **Table** : Collection de données en lignes et colonnes.
- **Schéma** : Organisation des tables et relations.
- **Clé primaire (PRIMARY KEY)** : Identifiant unique d'une table.
- **Clé étrangère (FOREIGN KEY)** : Relation entre tables.
- **Intégrité référentielle** : Cohérence des relations entre tables.

---

## Commandes SQL classifiées

### 1. Data Definition Language (DDL) - Langage de Définition de Données

Utilisé pour créer et modifier la structure des bases de données.

- **CREATE** : Création d'une base de données ou table
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
  ```
- **ALTER** : Modification d'une table
  ```sql
  ALTER TABLE etudiants ADD COLUMN adresse VARCHAR(255);
  ```
- **DROP** : Suppression d'une table
  ```sql
  DROP TABLE etudiants;
  ```
- **TRUNCATE** : Suppression de toutes les lignes sans supprimer la structure
  ```sql
  TRUNCATE TABLE etudiants;
  ```

---

### 2. Data Manipulation Language (DML) - Langage de Manipulation des Données

Utilisé pour insérer, modifier et supprimer des données.

- **INSERT** : Ajout de données
  ```sql
  INSERT INTO etudiants (nom, prenom, age, email) VALUES ('Dupont', 'Jean', 22, 'jean.dupont@email.com');
  ```
- **UPDATE** : Modification de données existantes
  ```sql
  UPDATE etudiants SET age = 23 WHERE id = 1;
  ```
- **DELETE** : Suppression de données
  ```sql
  DELETE FROM etudiants WHERE id = 1;
  ```

---

### 3. Data Query Language (DQL) - Langage de Requête des Données

Utilisé pour interroger la base de données.

- **SELECT** : Extraction de données
  ```sql
  SELECT * FROM etudiants;
  SELECT nom, prenom FROM etudiants WHERE age > 20;
  ```

---

### 4. Data Control Language (DCL) - Langage de Contrôle des Données

Utilisé pour accorder ou révoquer des permissions.

- **GRANT** : Donner des droits d'accès
  ```sql
  GRANT SELECT, INSERT ON gestion_ecole.* TO 'utilisateur'@'localhost';
  ```
- **REVOKE** : Retirer des droits d'accès
  ```sql
  REVOKE INSERT ON gestion_ecole.* FROM 'utilisateur'@'localhost';
  ```

---

### 5. Transaction Control Language (TCL) - Langage de Contrôle des Transactions

Utilisé pour gérer les transactions.

- **COMMIT** : Sauvegarde les modifications
  ```sql
  COMMIT;
  ```
- **ROLLBACK** : Annule les modifications en cours
  ```sql
  ROLLBACK;
  ```
- **SAVEPOINT** : Crée un point de sauvegarde dans une transaction
  ```sql
  SAVEPOINT point1;
  ```

---

## Jointures et optimisation des requêtes

### 1. Types de jointures

- **INNER JOIN** : Renvoie les enregistrements correspondants des deux tables
  ```sql
  SELECT etudiants.nom, cours.titre
  FROM etudiants
  INNER JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
  INNER JOIN cours ON inscriptions.cours_id = cours.id;
  ```
- **LEFT JOIN** : Renvoie tous les enregistrements de la table de gauche et ceux correspondants de la table de droite
  ```sql
  SELECT etudiants.nom, cours.titre
  FROM etudiants
  LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
  LEFT JOIN cours ON inscriptions.cours_id = cours.id;
  ```

### 2. Indexation pour optimiser les requêtes

L'indexation permet d'accélérer les recherches de données.
```sql
CREATE INDEX idx_email ON etudiants(email);
```

### 3. Bonnes pratiques d'optimisation

- Éviter `SELECT *`, préférer sélectionner uniquement les colonnes nécessaires.
- Utiliser des index sur les colonnes fréquemment utilisées dans `WHERE` ou `JOIN`.
- Supprimer les index inutiles pour réduire la charge sur les insertions/modifications.
- Utiliser des **vues (`VIEW`)** pour éviter de recalculer les résultats souvent sollicités.

