# **Chapitre 6 : Fonctionnement d'un serveur web**  

---

Un serveur web est un logiciel ou un matériel qui **stocke, traite et délivre** des pages web aux utilisateurs via un réseau, en utilisant des protocoles comme **HTTP/HTTPS**. Ce chapitre explore son architecture, les protocoles de communication, la configuration de serveurs populaires (**Apache, Nginx**) et la gestion des fichiers et permissions.

---

## **1. Architecture Client-Serveur**  

L’architecture **client-serveur** est un modèle de communication où un **client** envoie des requêtes à un **serveur**, qui lui renvoie des réponses.

### **1.1 Rôle du serveur web**
- Héberger et servir des pages web.
- Gérer les requêtes HTTP/HTTPS des clients.
- Traiter et exécuter des scripts (PHP, Node.js, etc.).
- Stocker et gérer des fichiers statiques (images, CSS, JS).

### **1.2 Fonctionnement d'une requête client-serveur**
1. L'utilisateur entre une URL dans son navigateur (`https://www.exemple.com`).
2. Le navigateur envoie une **requête HTTP** au serveur web.
3. Le serveur traite la requête et renvoie une **réponse HTTP** contenant la page demandée.
4. Le navigateur interprète et affiche la page à l'utilisateur.

🔍 **Exemple de requête HTTP :**  
```http
GET /index.html HTTP/1.1
Host: www.exemple.com
User-Agent: Mozilla/5.0
```

🔍 **Exemple de réponse HTTP :**  
```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>
  <body>
    <h1>Bienvenue sur mon site</h1>
  </body>
</html>
```

---

## **2. Protocoles HTTP et HTTPS**  

### **2.1 HTTP (HyperText Transfer Protocol)**
HTTP est un protocole de communication client-serveur sans état. Il utilise le port **80** par défaut.

#### **Méthodes HTTP principales :**
- `GET` : Récupérer une ressource (`GET /index.html`).
- `POST` : Envoyer des données (`POST /login` avec un formulaire).
- `PUT` : Mettre à jour une ressource.
- `DELETE` : Supprimer une ressource.

🔍 **Exemple de requête GET :**  
```http
GET /page.html HTTP/1.1
Host: www.exemple.com
```

---

### **2.2 HTTPS (HTTP Secure)**
HTTPS est une version sécurisée de HTTP qui utilise le chiffrement SSL/TLS. Il utilise le port **443**.

🔒 **Avantages :**
- Chiffrement des données (empêche l'interception des mots de passe).
- Authentification du serveur via des certificats SSL.
- Intégrité des données (évite les modifications frauduleuses).

🔍 **Exemple d’installation d’un certificat SSL avec Let’s Encrypt (sur Nginx) :**
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d exemple.com -d www.exemple.com
```

---

## **3. Configuration d'un serveur web (Apache, Nginx)**  

Deux serveurs web majeurs : **Apache** et **Nginx**.

### **3.1 Apache**
- Serveur web open-source populaire.
- Fonctionne avec un système de **modules** (PHP, SSL, etc.).
- Gère bien les **.htaccess** pour des règles personnalisées.

#### **Installation et configuration d’Apache :**
```bash
# Installation
sudo apt update
sudo apt install apache2

# Activer le service
sudo systemctl enable apache2
sudo systemctl start apache2

# Vérifier le statut
sudo systemctl status apache2
```

#### **Fichier de configuration Apache (`/etc/apache2/sites-available/000-default.conf`)**
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@exemple.com
    DocumentRoot /var/www/html
    ServerName exemple.com
    ServerAlias www.exemple.com

    <Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```
🔍 **Redémarrer Apache après modification :**  
```bash
sudo systemctl restart apache2
```

---

### **3.2 Nginx**
- Serveur web plus rapide et léger qu'Apache.
- Très performant en **reverse proxy** et **gestion des fichiers statiques**.
- Utilisé pour **équilibrer la charge** sur plusieurs serveurs.

#### **Installation et configuration de Nginx :**
```bash
# Installation
sudo apt update
sudo apt install nginx

# Activer le service
sudo systemctl enable nginx
sudo systemctl start nginx

# Vérifier le statut
sudo systemctl status nginx
```

#### **Fichier de configuration Nginx (`/etc/nginx/sites-available/exemple`)**
```nginx
server {
    listen 80;
    server_name exemple.com www.exemple.com;
    root /var/www/html;
    index index.html index.php;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
```
🔍 **Redémarrer Nginx après modification :**  
```bash
sudo systemctl restart nginx
```

---

## **4. Gestion des fichiers et permissions**  

Un serveur web doit gérer correctement les fichiers et leurs permissions pour éviter les erreurs et les failles de sécurité.

### **4.1 Arborescence typique d’un serveur web**
```
/var/www/              # Dossier racine du serveur
    ├── html/         # Contient les fichiers du site
    │   ├── index.html
    │   ├── style.css
    │   ├── script.js
    ├── logs/         # Contient les logs du serveur
    │   ├── access.log
    │   ├── error.log
```

---

### **4.2 Permissions des fichiers**
Il est important de bien gérer les permissions pour éviter les accès non autorisés.

#### **Commandes utiles :**
```bash
# Donner la propriété des fichiers au serveur web (www-data pour Apache/Nginx sous Linux)
sudo chown -R www-data:www-data /var/www/html

# Définir les permissions :
# 755 (lecture/écriture/exécution pour le propriétaire, lecture/exécution pour les autres)
sudo chmod -R 755 /var/www/html

# 644 (lecture/écriture pour le propriétaire, lecture seule pour les autres)
sudo chmod 644 /var/www/html/index.html
```

---

### **4.3 Logs et monitoring**
Les logs permettent de surveiller l'activité du serveur et de détecter les erreurs.

🔍 **Fichiers de logs principaux :**
- **Apache** : `/var/log/apache2/access.log` et `/var/log/apache2/error.log`
- **Nginx** : `/var/log/nginx/access.log` et `/var/log/nginx/error.log`

✍️ **Vérifier les erreurs en direct :**
```bash
tail -f /var/log/nginx/error.log
```

---
