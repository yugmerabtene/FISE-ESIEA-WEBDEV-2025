# **Chapitre 7 : Achat de domaine et hébergement**  

---

L’achat d’un **nom de domaine** et le choix d’un **hébergeur web** sont des étapes clés pour mettre un site en ligne. Ce chapitre couvre l’enregistrement d’un domaine, la comparaison des hébergeurs, la configuration DNS et la sécurité des domaines.

---

## **1. Choix et enregistrement d’un nom de domaine**  

Un **nom de domaine** est l’adresse web utilisée pour accéder à un site (ex : `www.exemple.com`). Il est associé à une **adresse IP** pour identifier le serveur web.  

### **1.1 Critères de choix d’un nom de domaine**
🔹 **Facilité de mémorisation** : Un nom court et simple à retenir.  
🔹 **Extension pertinente** (`.com`, `.fr`, `.net`, `.org`, etc.).  
🔹 **Disponibilité** : Vérifier que le nom n’est pas déjà pris.  
🔹 **SEO (Référencement)** : Un nom pertinent peut aider au positionnement sur Google.  

### **1.2 Processus d’achat d’un domaine**
1. **Vérifier la disponibilité** avec un outil comme [Whois](https://who.is/) ou chez un **registrar** (ex : OVH, Gandi, Namecheap).  
2. **Choisir l’extension** (`.com` pour l’international, `.fr` pour la France, `.tech` pour la technologie, etc.).  
3. **Acheter et enregistrer** le domaine en fournissant ses coordonnées.  
4. **Configurer le DNS** pour pointer le domaine vers le serveur web.  

🔍 **Exemple d’achat sur OVH :**
1. Aller sur [www.ovh.com](https://www.ovh.com).  
2. Taper le nom de domaine souhaité.  
3. Sélectionner l’extension et valider l’achat.  
4. Configurer les **serveurs DNS** et **redirections** après l’achat.  

---

## **2. Comparaison des hébergeurs web**  

Un **hébergeur web** stocke les fichiers de votre site et les met en ligne. Il existe plusieurs types d’hébergements :

### **2.1 Types d’hébergements**
| Type d’hébergement | Avantages | Inconvénients |
|-------------------|-----------|---------------|
| **Hébergement mutualisé** | Pas cher, facile à gérer | Performances limitées, mutualisation des ressources |
| **VPS (Serveur Virtuel Privé)** | Plus de contrôle, personnalisable | Requiert des connaissances techniques |
| **Serveur dédié** | Performances élevées, aucun partage | Coût élevé, gestion complexe |
| **Cloud (AWS, Google Cloud, Azure)** | Évolutif, haute disponibilité | Facturation à l’usage, complexité |

### **2.2 Comparatif des meilleurs hébergeurs**
| Hébergeur | Type | Points forts | Prix (approximatif) |
|-----------|------|-------------|---------------------|
| **OVH** | Mutualisé, VPS, Dédié | Serveurs en Europe, bon rapport qualité/prix | Dès 2,99€/mois |
| **Hostinger** | Mutualisé, VPS | Interface simple, tarifs compétitifs | Dès 1,99€/mois |
| **Bluehost** | Mutualisé, VPS, Dédié | Idéal pour WordPress | Dès 2,95$/mois |
| **Infomaniak** | Mutualisé, VPS | Écologique, basé en Suisse | Dès 5,75€/mois |
| **AWS (Amazon Web Services)** | Cloud | Très puissant, adapté aux grandes entreprises | Tarification à l’usage |

🔍 **Exemple d’achat d’hébergement sur OVH :**
1. Aller sur [www.ovh.com](https://www.ovh.com).  
2. Sélectionner un plan d’hébergement.  
3. Associer un nom de domaine.  
4. Finaliser le paiement et accéder à l’espace client.  

---

## **3. Configuration DNS et gestion des serveurs**  

Le **DNS (Domain Name System)** traduit un nom de domaine (`www.exemple.com`) en une **adresse IP** pour identifier le serveur.

### **3.1 Les principaux enregistrements DNS**
| Type d’enregistrement | Fonction |
|----------------------|----------|
| **A** | Associe un domaine à une adresse IP (IPv4) |
| **AAAA** | Associe un domaine à une adresse IPv6 |
| **CNAME** | Alias qui pointe vers un autre nom de domaine |
| **MX** | Définit le serveur de messagerie (ex : Gmail, Outlook) |
| **TXT** | Stocke des informations textuelles (ex : SPF pour les emails) |

🔍 **Exemple : Configurer un domaine pour pointer vers un serveur**
1. Aller dans l’interface DNS de votre hébergeur.  
2. Ajouter un **enregistrement A** :  
   - Nom : `@` (racine)  
   - Type : `A`  
   - Valeur : `192.168.1.1` (adresse IP du serveur)  
3. Ajouter un **CNAME** pour `www.exemple.com` pointant vers `exemple.com`.  

---

### **3.2 Hébergement sur un VPS (Exemple avec Apache)**
1. **Acheter un VPS** (OVH, Digital Ocean, etc.).  
2. **Accéder au serveur en SSH** :  
   ```bash
   ssh user@IP_du_serveur
   ```
3. **Installer Apache/Nginx** :  
   ```bash
   sudo apt update
   sudo apt install apache2
   ```
4. **Créer un Virtual Host pour le domaine** :
   ```bash
   sudo nano /etc/apache2/sites-available/exemple.com.conf
   ```
   ```apache
   <VirtualHost *:80>
       ServerName exemple.com
       ServerAlias www.exemple.com
       DocumentRoot /var/www/exemple
   </VirtualHost>
   ```
5. **Activer et redémarrer Apache** :  
   ```bash
   sudo a2ensite exemple.com.conf
   sudo systemctl restart apache2
   ```

---

## **4. Sécurité et renouvellement des domaines**  

Un domaine doit être protégé contre **le vol, l’expiration et les cyberattaques**.

### **4.1 Sécuriser un nom de domaine**
✅ **Activer le verrouillage du domaine** : Empêche les transferts non autorisés.  
✅ **Utiliser un certificat SSL** : Sécurise la connexion (`https://`).  
✅ **Activer la protection WHOIS** : Masque les informations personnelles du propriétaire.  

🔍 **Exemple : Activer la protection WHOIS sur Namecheap**
1. Aller sur votre compte Namecheap.  
2. Sélectionner votre domaine.  
3. Activer l’option **WhoisGuard**.  

---

### **4.2 Renouvellement et expiration**
- **Renouvellement automatique** : Évite la perte du domaine.  
- **Période de grâce** : Certains registrars offrent quelques jours après expiration pour récupérer un domaine.  
- **Récupération après expiration** : Après la période de grâce, le domaine peut être racheté par quelqu’un d’autre.  

🔍 **Vérifier la date d’expiration d’un domaine**
```bash
whois exemple.com
```

---
