## **Chapitre 1 : Versioning et DevOps CI/CD (Important sur GitHub)**

---

### **1. Versioning**
Le versioning est une pratique essentielle dans le développement logiciel qui permet de suivre les modifications apportées au code source, de collaborer efficacement et d'assurer la stabilité du projet.

#### **1.1 Introduction aux systèmes de contrôle de version**
Un système de contrôle de version (VCS - Version Control System) est un outil qui enregistre et gère les différentes versions d'un fichier ou d'un ensemble de fichiers.

##### **Types de systèmes de contrôle de version**
- **VCS centralisé (CVCS)** : Un seul serveur central stocke toutes les versions du code (exemple : SVN - Subversion).
- **VCS distribué (DVCS)** : Chaque utilisateur possède une copie complète du dépôt, ce qui permet un meilleur travail en autonomie (exemple : Git, Mercurial).

Parmi ces systèmes, **Git** est le plus populaire aujourd’hui.

---

#### **1.2 Concepts de base de Git**
Git fonctionne en **suivant l’historique des modifications** sous forme de snapshots et utilise des commandes spécifiques pour gérer les versions.

✅ **Quelques concepts clés :**
- **Commit** : Une capture instantanée des modifications d’un fichier.
- **Branch (branche)** : Une version parallèle du code permettant de travailler sans affecter la branche principale.
- **Merge (fusion)** : L’intégration des changements d’une branche dans une autre.
- **Rebase** : Réappliquer les commits d’une branche sur une autre en modifiant leur base.
- **Pull & Push** : Récupération et envoi des modifications vers un dépôt distant.

✍️ **Exemple d’utilisation :**
```bash
# Initialiser un dépôt Git
git init

# Ajouter des fichiers au suivi
git add .

# Enregistrer les modifications
git commit -m "Premier commit"

# Créer une nouvelle branche
git branch feature-1

# Passer sur la branche feature-1
git checkout feature-1

# Fusionner les modifications dans la branche principale
git checkout main
git merge feature-1
```

---

#### **1.3 Bonnes pratiques pour le versioning de code**
- **Utiliser des messages de commit clairs et descriptifs** (Ex: `git commit -m "Ajout de la fonctionnalité d'authentification"`).
- **Adopter un modèle de branchement** comme **Git Flow** ou **GitHub Flow**.
- **Éviter de committer des fichiers temporaires** en utilisant un `.gitignore`.
- **Faire des pull requests (PR) pour valider le code avant de le fusionner.**
- **Utiliser des tags pour marquer les versions importantes** (`git tag -a v1.0 -m "Version stable"`).

---

#### **1.4 Outils de gestion de dépôts**
Git peut être utilisé avec différentes plateformes :
- **GitHub** : La plus populaire, avec des outils de collaboration et CI/CD intégrés.
- **GitLab** : Solution DevOps complète avec intégration CI/CD native.
- **Bitbucket** : Intégré avec Jira et souvent utilisé dans les entreprises.

Chaque plateforme propose des outils comme la gestion des Pull Requests, le suivi des issues et l'intégration avec CI/CD.

---

### **2. DevOps CI/CD**
Le DevOps est une approche visant à améliorer la collaboration entre les équipes de développement (Dev) et les opérations (Ops) en automatisant les processus de livraison et de déploiement.

---

#### **2.1 Introduction à l’intégration continue et au déploiement continu (CI/CD)**
Le **CI/CD (Continuous Integration / Continuous Deployment)** repose sur deux piliers :
- **Intégration continue (CI)** : Tester et valider automatiquement le code à chaque modification.
- **Déploiement continu (CD)** : Déployer automatiquement les mises à jour validées.

🚀 **Avantages :**
- Réduction des erreurs grâce aux tests automatisés.
- Livraison rapide des nouvelles fonctionnalités.
- Réduction des conflits entre développeurs.

---

#### **2.2 Pipeline CI/CD : Construction, Test, Déploiement**
Un pipeline CI/CD est un ensemble d’étapes automatisées pour livrer du code.

🔗 **Phases d’un pipeline CI/CD :**
1. **Build (Construction du code)**
   - Compilation du code.
   - Téléchargement des dépendances.
   
2. **Test (Validation du code)**
   - Tests unitaires.
   - Tests d'intégration.

3. **Déploiement (Mise en production)**
   - Déploiement sur un serveur de test.
   - Déploiement en production après validation.

📌 **Exemple de pipeline GitHub Actions :**
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout du code
        uses: actions/checkout@v4

      - name: Installation des dépendances
        run: npm install

      - name: Exécution des tests
        run: npm test

      - name: Déploiement
        run: echo "Déploiement en cours..."
```

---

#### **2.3 Outils CI/CD**
Plusieurs outils permettent d’automatiser les pipelines CI/CD :
- **GitHub Actions** : Solution intégrée à GitHub pour automatiser les tests et les déploiements.
- **Jenkins** : Outil open-source flexible pour CI/CD.
- **GitLab CI/CD** : Intégré à GitLab pour gérer les pipelines de bout en bout.
- **CircleCI & Travis CI** : Solutions cloud pour automatiser les tests et les livraisons.

---

#### **2.4 Automatisation des tests et des déploiements**
L’automatisation est un élément clé du DevOps. Voici les principales approches :
- **Tests automatisés** : Exécuter automatiquement des tests unitaires et d’intégration.
- **Infrastructure as Code (IaC)** : Utiliser Terraform ou Ansible pour gérer les infrastructures.
- **Déploiements continus** : Automatiser la mise en production avec Kubernetes, Docker et Helm.

📌 **Exemple de tests automatisés avec Jest :**
```javascript
test('addition de 2 + 2', () => {
  expect(2 + 2).toBe(4);
}); 
```