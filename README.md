# 🐳 Docker

---

# 1. Qu'est-ce que Docker ?

* **Docker** est une plateforme open-source permettant de créer, déployer et exécuter des applications dans des **conteneurs**.
* Un conteneur embarque :

  * le code,
  * les dépendances,
  * les bibliothèques,
  * la configuration nécessaire au fonctionnement de l'application.

## ✅ Avantages de Docker

* Isolation des applications et de leurs dépendances.
* Portabilité entre les environnements (dev, test, production).
* Léger comparé aux machines virtuelles.
* Démarrage rapide des services.

---

# 2. Concepts Clés

## 📦 Image Docker

Une **image Docker** est un modèle immuable utilisé pour créer des conteneurs.

### Caractéristiques

* Contient :

  * le système de base,
  * les dépendances,
  * le code source,
  * les configurations.
* Stockée dans un **registry** :

  * Docker Hub,
  * GitLab Registry,
  * GitHub Container Registry.

### Exemples

```bash
nginx:latest
ubuntu:22.04
```

---

## 🐳 Conteneur Docker

Un **conteneur** est une instance exécutable d'une image Docker.

### Cycle de vie

```text
Créé → Démarré → Arrêté → Supprimé
```

### Particularités

* Isolé des autres conteneurs.
* Partage le noyau de l’OS hôte.
* Peut être démarré ou supprimé rapidement.

---

## 📜 Dockerfile

Le **Dockerfile** est un fichier texte contenant les instructions permettant de construire une image Docker.

### Exemple simple

```dockerfile
FROM ubuntu:22.04

RUN apt update && apt install -y python3

COPY app.py /app/

WORKDIR /app

CMD ["python3", "app.py"]
```

### Explication des instructions

| Instruction | Description                           |
| ----------- | ------------------------------------- |
| `FROM`      | Définit l’image de base               |
| `RUN`       | Exécute une commande pendant le build |
| `COPY`      | Copie des fichiers dans l’image       |
| `WORKDIR`   | Définit le répertoire de travail      |
| `CMD`       | Commande exécutée au démarrage        |

---

# 3. Commandes Essentielles

## 🔹 Gestion des Images

| Commande                        | Description                               |
| ------------------------------- | ----------------------------------------- |
| `docker pull <image>`           | Télécharger une image depuis un registry  |
| `docker build -t <nom-image> .` | Construire une image depuis un Dockerfile |
| `docker images`                 | Lister les images locales                 |
| `docker rmi <image>`            | Supprimer une image                       |

### Exemple

```bash
docker pull nginx
docker build -t mon-app .
docker images
docker rmi mon-app
```

---

## 🔹 Gestion des Conteneurs

| Commande                                | Description                           |
| --------------------------------------- | ------------------------------------- |
| `docker run [options] <image>`          | Créer et démarrer un conteneur        |
| `docker run -d --name <nom> <image>`    | Démarrer un conteneur en arrière-plan |
| `docker ps`                             | Lister les conteneurs actifs          |
| `docker ps -a`                          | Lister tous les conteneurs            |
| `docker stop <conteneur>`               | Arrêter un conteneur                  |
| `docker start <conteneur>`              | Redémarrer un conteneur arrêté        |
| `docker rm <conteneur>`                 | Supprimer un conteneur                |
| `docker exec -it <conteneur> /bin/bash` | Ouvrir un shell dans un conteneur     |

### Exemple

```bash
docker run -d --name web nginx
docker ps
docker stop web
docker start web
docker exec -it web /bin/bash
```

---

# 4. Docker Compose

## 📌 Qu'est-ce que Docker Compose ?

Docker Compose permet de définir et lancer plusieurs conteneurs via un fichier YAML (`docker-compose.yml`).

## ✅ Avantages

* Gestion simplifiée des architectures multi-services.
* Configuration centralisée.
* Mise en réseau automatique des services.
* Très utile pour le développement local.

### Exemple d’architecture

```text
Frontend ↔ Backend ↔ Base de données
```

---

# 📌 Concepts Clés de Docker Compose

| Concept       | Description                            |
| ------------- | -------------------------------------- |
| `service`     | Conteneur applicatif                   |
| `network`     | Réseau de communication entre services |
| `volume`      | Stockage persistant                    |
| `environment` | Variables d’environnement              |

---

# 📌 Exemple de docker-compose.yml

```yaml
version: "3.8"

services:

  # Service Web
  web:
    build: .

    ports:
      - "5000:5000"

    volumes:
      - .:/app

    environment:
      - FLASK_ENV=development

    depends_on:
      - redis

  # Service Redis
  redis:
    image: "redis:alpine"

    ports:
      - "6379:6379"

    volumes:
      - redis_data:/data

volumes:
  redis_data:
```

---

## 🔹 Explication du fichier

### Service `web`

* Construit l’image localement.
* Expose le port `5000`.
* Monte le dossier courant dans le conteneur.
* Attend le service Redis.

### Service `redis`

* Utilise une image officielle Redis légère.
* Sauvegarde les données dans un volume persistant.

---

# 🔹 Commandes Docker Compose

| Commande                             | Description                           |
| ------------------------------------ | ------------------------------------- |
| `docker-compose up -d`               | Démarrer les services                 |
| `docker-compose down`                | Arrêter et supprimer les services     |
| `docker-compose ps`                  | Voir les services actifs              |
| `docker-compose logs`                | Afficher les logs                     |
| `docker-compose logs <service>`      | Logs d’un service spécifique          |
| `docker-compose build`               | Rebuild des images                    |
| `docker-compose restart <service>`   | Redémarrer un service                 |
| `docker-compose exec <service> bash` | Exécuter une commande dans un service |

### Exemple

```bash
docker-compose up -d
docker-compose ps
docker-compose logs web
docker-compose down
```

---

# 5. Exemple Complet de Dockerfile

```dockerfile
# Image de base
FROM python:3.9-slim

# Répertoire de travail
WORKDIR /app

# Copie des dépendances
COPY requirements.txt .

# Installation des dépendances
RUN pip install --no-cache-dir -r requirements.txt

# Copie du code source
COPY . .

# Commande de lancement
CMD ["python", "app.py"]
```

---

# 📌 Bonnes Pratiques Docker

## ✅ Utiliser des images légères

Préférer :

```dockerfile
python:3.9-slim
alpine
```

## ✅ Utiliser des volumes

Pour :

* les bases de données,
* les uploads,
* les données persistantes.

---

# 📚 Résumé

| Élément        | Rôle                           |
| -------------- | ------------------------------ |
| Image          | Modèle de conteneur            |
| Conteneur      | Instance exécutable            |
| Dockerfile     | Instructions de build          |
| Docker Compose | Orchestration multi-conteneurs |
| Volume         | Persistance des données        |
| Network        | Communication entre services   |

---
