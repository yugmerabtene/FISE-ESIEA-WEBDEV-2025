### TP-03-SYS-RESERV-CAL : Système de Réservation avec Calendrier

#### **Cahier des Charges**

---

### **Descriptif du Projet**

Le projet consiste à développer un système de réservation en ligne avec un calendrier intégré. Ce système permettra aux utilisateurs de créer un compte, de prendre des rendez-vous, de gérer leurs informations personnelles, et de vérifier la disponibilité des créneaux horaires. Le système doit être sécurisé, facile à utiliser, et respecter les contraintes techniques et fonctionnelles définies.

---

### **Descriptif Fonctionnel**

1. **Gestion des Utilisateurs** : 
   - **Création de compte** : (2 points)  
     - Formulaire d'inscription avec nom, prénom, date de naissance, adresse postale, numéro de téléphone, email, et mot de passe.
     - Vérification de l'unicité de l'email.
     - Envoi d'un email de vérification pour activer le compte.
   - **Connexion et déconnexion** : (2 points)  
     - Formulaire de connexion avec email et mot de passe.
     - Redirection vers le profil après connexion réussie.
   - **Modification des informations personnelles** :  (2 points)
     - Possibilité de modifier le nom, prénom, date de naissance, adresse postale, numéro de téléphone, et email.
     - Vérification de l'unicité de l'email lors de la modification.
   - **Suppression du compte** :  (2 points)
     - Possibilité de supprimer le compte.
     - Suppression de toutes les données associées en base de données.

2. **Gestion des Rendez-vous** :
   - **Prise de rendez-vous** : (2 points)  
     - Calendrier interactif pour la sélection des dates et heures.
     - Vérification de la disponibilité des créneaux horaires.
     - Enregistrement du rendez-vous si le créneau est disponible.
   - **Affichage des rendez-vous** : (2 points)  
     - Affichage des rendez-vous pris par l'utilisateur.
   - **Annulation de rendez-vous** :  (2 points)   
     - Possibilité d'annuler un rendez-vous pris.
     - Libération du créneau horaire pour d'autres utilisateurs.

3. **Sécurité** :
   - **Protection contre les attaques CSRF** : (1 points)
     - Ajout de tokens CSRF pour sécuriser les formulaires.
   - **Hachage des mots de passe** :  (1 points)
     - Utilisation de `password_hash()` pour hasher les mots de passe avant de les stocker.
   - **Vérification de l'unicité de l'email** :  (1 points)
     - Vérification de l'unicité de l'email lors de l'inscription et de la modification du profil.
   - **Protection contre les attaques XSS et SQL Injection** : (1 points)

4. **Interface Utilisateur** :
   - **Calendrier interactif** :  (1 points)
     - Interface utilisateur intuitive pour la sélection des dates et heures.
   - **Formulaire de contact** :  (1 points)
     - Formulaire pour les demandes de renseignements.

---

### **Contraintes Techniques**

1. **Technologies** :
   - Frontend : Bootstrap.
   - Backend : PHP procédural.
   - Base de données : MySQL.

2. **Sécurité** :
   - Protection contre les attaques XSS et SQL Injection.
   - Utilisation de tokens CSRF pour les formulaires.
   - Validation des emails et des numéros de téléphone.

3. **Performance** :
   - Gestion efficace des créneaux horaires pour éviter les conflits de rendez-vous.
   - Optimisation des requêtes SQL pour une meilleure performance.

---

### **User Stories**

1. **En tant qu'utilisateur, je veux pouvoir créer un compte** :
   - **Cas d'acceptation** :
     - Le formulaire d'inscription demande un nom, un prénom, une date de naissance, une adresse postale, un numéro de téléphone, un email, et un mot de passe.
     - L'email doit être unique.
     - Un email de vérification est envoyé pour activer le compte.

2. **En tant qu'utilisateur, je veux pouvoir me connecter** :
   - **Cas d'acceptation** :
     - Le formulaire de connexion demande un email et un mot de passe.
     - Si les informations sont correctes, l'utilisateur est redirigé vers son profil.

3. **En tant qu'utilisateur, je veux pouvoir modifier mes informations personnelles** :
   - **Cas d'acceptation** :
     - L'utilisateur peut modifier son nom, prénom, date de naissance, adresse postale, numéro de téléphone, et email.
     - L'email doit être unique.

4. **En tant qu'utilisateur, je veux pouvoir prendre un rendez-vous** :
   - **Cas d'acceptation** :
     - L'utilisateur peut sélectionner une date et une heure dans un calendrier interactif.
     - Le système vérifie la disponibilité du créneau horaire.
     - Si le créneau est disponible, le rendez-vous est enregistré.

5. **En tant qu'utilisateur, je veux pouvoir annuler un rendez-vous** :
   - **Cas d'acceptation** :
     - L'utilisateur peut annuler un rendez-vous pris.
     - Le créneau horaire est libéré et disponible pour d'autres utilisateurs.

6. **En tant qu'utilisateur, je veux pouvoir supprimer mon compte** :
   - **Cas d'acceptation** :
     - L'utilisateur peut supprimer son compte.
     - Toutes les données associées à l'utilisateur sont supprimées de la base de données.

---

### **Livrables**

1. **Use Case** :
   - Description détaillée des cas d'utilisation avec les acteurs et les scénarios.

2. **Diagramme de Séquence** :
   - Diagramme illustrant les interactions entre l'utilisateur et le système pour les principales fonctionnalités (création de compte, prise de rendez-vous, modification des informations personnelles, suppression de compte).

3. **Schéma de Base de Données** :
   - Schéma illustrant les tables et leurs relations avec les cardinalités.

4. **Projet Versionné** :
   - Projet versionné avec des commits réguliers et clairs.
