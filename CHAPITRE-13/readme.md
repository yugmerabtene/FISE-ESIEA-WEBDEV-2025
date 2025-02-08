# Chapitre 13 : Bases de Données avec MySQL

## Introduction aux bases de données relationnelles

### Qu'est-ce qu'une base de données relationnelle ?
Une base de données relationnelle est un ensemble de données structurées en tables, où les relations entre les données sont définies par des clés. MySQL est un **Système de Gestion de Base de Données Relationnelle (SGBDR)** qui permet de stocker, manipuler et gérer ces données efficacement.

### Concepts clés des bases de données relationnelles
- **Table** : Une collection de données organisée en lignes et colonnes.
- **Schéma** : Structure définissant l'organisation des tables et leurs relations.
- **Clé primaire (PRIMARY KEY)** : Un identifiant unique pour chaque enregistrement d'une table.
- **Clé étrangère (FOREIGN KEY)** : Un champ qui établit une relation entre deux tables.
- **Intégrité référentielle** : Garantie que les relations entre tables sont cohérentes.

---

## Création et gestion de tables

### Création d'une base de données
Avant de créer des tables, il faut créer une base de données :
```sql
CREATE DATABASE gestion_ecole;
USE gestion_ecole;
```

### Création d'une table
Créons une table **"etudiants"** :
```sql
CREATE TABLE etudiants (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18),
    email VARCHAR(100) UNIQUE
);
```
Explication :
- `AUTO_INCREMENT` : Incrémente automatiquement la clé primaire.
- `NOT NULL` : Champ obligatoire.
- `CHECK (age >= 18)` : Contrainte sur l'âge.
- `UNIQUE` : Empêche les doublons.

### Modification et suppression d'une table
- Ajouter une colonne :
  ```sql
  ALTER TABLE etudiants ADD COLUMN adresse VARCHAR(255);
  ```
- Supprimer une colonne :
  ```sql
  ALTER TABLE etudiants DROP COLUMN adresse;
  ```
- Supprimer une table :
  ```sql
  DROP TABLE etudiants;
  ```

---

## Requêtes SQL de base (SELECT, INSERT, UPDATE, DELETE)

### Insérer des données
```sql
INSERT INTO etudiants (nom, prenom, age, email) VALUES ('Dupont', 'Jean', 22, 'jean.dupont@email.com');
```

### Sélectionner des données
```sql
SELECT * FROM etudiants;
SELECT nom, prenom FROM etudiants WHERE age > 20;
```

### Mettre à jour des données
```sql
UPDATE etudiants SET age = 23 WHERE id = 1;
```

### Supprimer des données
```sql
DELETE FROM etudiants WHERE id = 1;
```

---

## Jointures et optimisation des requêtes

### Les types de jointures

1. **Jointure interne (INNER JOIN)** : Retourne les enregistrements correspondants dans les deux tables.
   ```sql
   SELECT etudiants.nom, cours.titre
   FROM etudiants
   INNER JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
   INNER JOIN cours ON inscriptions.cours_id = cours.id;
   ```

2. **Jointure externe gauche (LEFT JOIN)** : Retourne tous les enregistrements de la table de gauche, et les correspondances de la table de droite.
   ```sql
   SELECT etudiants.nom, cours.titre
   FROM etudiants
   LEFT JOIN inscriptions ON etudiants.id = inscriptions.etudiant_id
   LEFT JOIN cours ON inscriptions.cours_id = cours.id;
   ```

### Indexation pour optimiser les requêtes
L'indexation accélère la recherche des données :
```sql
CREATE INDEX idx_email ON etudiants(email);
```

### Bonnes pratiques d'optimisation
- Éviter `SELECT *` et sélectionner uniquement les colonnes nécessaires.
- Utiliser des index sur les colonnes souvent utilisées dans **WHERE** ou **JOIN**.
- Supprimer les index inutiles pour réduire la charge sur les insertions/modifications.
- Préférer les **vues** (`VIEW`) pour éviter de recalculer des résultats fréquemment utilisés.

