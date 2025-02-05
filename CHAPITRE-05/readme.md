# **Chapitre 5 : Analyse et Conception**  

---

L'analyse et la conception sont des étapes fondamentales dans le développement logiciel. Elles permettent de structurer un projet avant son implémentation. Ce chapitre explore **UML**, **Merise** et le **design d'interface**.

---

## **1. UML (Unified Modeling Language)**
UML est un langage graphique permettant de modéliser un système avant de le coder. Il offre plusieurs types de diagrammes pour visualiser la structure et le comportement du système.

### **1.1 Diagramme de cas d'utilisation (Use Case)**
Le **diagramme de cas d'utilisation** représente les interactions entre les utilisateurs (acteurs) et le système.

🔹 **Éléments clés :**  
- **Acteur** : Un utilisateur ou un autre système interagissant avec l'application.  
- **Cas d'utilisation** : Une action réalisée par le système (ex : "Créer un compte").  
- **Relations** : Liens entre acteurs et cas d'utilisation (`association`, `inclusion`, `extension`).  

✍️ **Exemple de diagramme de cas d'utilisation (Système de réservation de vols)**  
```
  Utilisateur --> [Rechercher un vol]
  Utilisateur --> [Réserver un billet]
  Utilisateur --> [Annuler une réservation]
```
📌 **Utilisation :** Ce diagramme est souvent utilisé pour capturer les besoins fonctionnels.

---

### **1.2 Diagramme de classes**
Le **diagramme de classes** représente la structure du système en mettant en évidence les **objets**, leurs **attributs** et leurs **relations**.

🔹 **Éléments clés :**  
- **Classes** : Définitions des objets (ex : `Client`, `Commande`).  
- **Attributs** : Caractéristiques des classes (ex : `nom`, `email`).  
- **Méthodes** : Actions que les classes peuvent effectuer (ex : `passerCommande()`).  
- **Relations** : Associations entre classes (`héritage`, `association`, `composition`).  

✍️ **Exemple :**
```
Classe Client {
  nom : String
  email : String
  passerCommande()
}
Classe Commande {
  numéro : Int
  date : Date
}
Client -- Commande  (relation : "Passe")
```
📌 **Utilisation :** Utile pour modéliser la structure des bases de données ou du code.

---

### **1.3 Diagramme de séquence**
Le **diagramme de séquence** montre l'échange de messages entre les objets dans le temps.

🔹 **Éléments clés :**  
- **Acteurs et objets**  
- **Flèche de communication** (Appel de méthodes)  
- **Chronologie des interactions**  

✍️ **Exemple (Processus de connexion) :**
```
Utilisateur -> Système : Entrer identifiants
Système -> Base de données : Vérifier login
Base de données -> Système : Renvoie résultat
Système -> Utilisateur : Confirmation
```
📌 **Utilisation :** Visualisation des scénarios d'interaction.

---

### **1.4 Diagramme d'activité**
Le **diagramme d'activité** modélise le **flux d’exécution** des actions.

🔹 **Éléments clés :**  
- **Activités (actions exécutées)**  
- **Flèches de transition (logique de progression)**  
- **Branches conditionnelles (if/else)**  

✍️ **Exemple (Processus d’achat en ligne) :**
```
Début -> Sélectionner un produit -> Ajouter au panier -> [Paiement validé ?] -> Oui -> Confirmer commande -> Fin
                                            |
                                            Non
                                            |
                                          Annuler
```
📌 **Utilisation :** Représenter les processus métier.

---

## **2. Méthode Merise (Modélisation des Données)**
Merise est une méthode d'analyse pour concevoir une base de données à travers trois modèles : **MCD, MLD et MPD**.

### **2.1 Modèle Conceptuel de Données (MCD)**
Le **MCD** représente les données sans notion technique.

🔹 **Éléments clés :**  
- **Entités** (ex : `Client`, `Commande`).  
- **Associations** (ex : `Un client passe plusieurs commandes`).  
- **Cardinalités** (`1,1` ou `0,n` indiquant le nombre de relations possibles).  

✍️ **Exemple :**
```
[Client] 1 -- n [Commande]
```
📌 **Utilisation :** Comprendre les relations entre entités.

---

### **2.2 Modèle Logique de Données (MLD)**
Le **MLD** traduit le MCD en tables.

✍️ **Exemple :**
```sql
Table Client (
  id_client INT PRIMARY KEY,
  nom VARCHAR(50),
  email VARCHAR(100)
);

Table Commande (
  id_commande INT PRIMARY KEY,
  date DATE,
  id_client INT FOREIGN KEY REFERENCES Client(id_client)
);
```
📌 **Utilisation :** Étape avant l’implémentation.

---

### **2.3 Modèle Physique de Données (MPD)**
Le **MPD** précise les contraintes techniques (index, types de données).

✍️ **Exemple :**
```sql
CREATE INDEX idx_client_email ON Client(email);
```
📌 **Utilisation :** Optimiser les performances.

---

## **3. Design d’interface**
Le design d'interface vise à créer des interfaces intuitives et ergonomiques.

### **3.1 Wireframe (Schéma fonctionnel)**
Un **wireframe** est un **squelette visuel** d’une interface sans graphisme avancé.

📌 **Utilisation :** Définir la structure avant le design final.  
📌 **Outils :** Balsamiq, Figma.

---

### **3.2 Maquettage**
Le **maquettage** ajoute des éléments graphiques.

📌 **Utilisation :** Présenter un aperçu réaliste.  
📌 **Outils :** Adobe XD, Sketch.

---

### **3.3 Prototypage**
Le **prototypage** intègre des interactions (boutons cliquables, animations).

📌 **Utilisation :** Tester l’expérience utilisateur.  
📌 **Outils :** Figma, InVision.



