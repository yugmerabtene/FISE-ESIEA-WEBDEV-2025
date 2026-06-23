## Chapitre 19 : Déploiement et Conteneurisation

### 1. Introduction à Docker
Docker est une plateforme de conteneurisation qui permet d'emballer, distribuer et exécuter des applications dans des environnements isolés appelés conteneurs.

### 2. Commandes Docker essentielles
```bash
# Lister les conteneurs en cours d'exécution
docker ps

# Construire une image Docker
docker build -t mon-image .

# Exécuter un conteneur
docker run -d -p 8080:80 mon-image
```

### 3. Docker Compose
Docker Compose permet de définir et exécuter des applications multi-conteneurs.

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "8080:80"
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
```

### 4. Déploiement sur un serveur
- Utilisation de GitHub Actions pour déployer automatiquement
- Configuration d'un VPS (Ubuntu + Nginx + Docker)
- Mise en place d'un nom de domaine et SSL avec Let's Encrypt

---

*Documentation en cours de rédaction.*
