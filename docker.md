# DOCKER
---

### 🚀 **Commandes de Base Docker**

- **Lister les conteneurs actifs :**
  ```bash
  docker ps
  ```
- **Lister tous les conteneurs (actifs et inactifs) :**
  ```bash
  docker ps -a
  ```
- **Démarrer un conteneur :**
  ```bash
  docker start <nom_du_conteneur_ou_ID>
  ```
- **Arrêter un conteneur :**
  ```bash
  docker stop <nom_du_conteneur_ou_ID>
  ```
- **Supprimer un conteneur :**
  ```bash
  docker rm <nom_du_conteneur_ou_ID>
  ```
- **Supprimer une image :**
  ```bash
  docker rmi <ID_de_l_image>
  ```

---

### 📦 **Gestion des Images**

- **Télécharger une image depuis Docker Hub :**
  ```bash
  docker pull <nom_image>:<tag>
  ```
  *Exemple :* `docker pull nginx:latest`
- **Créer une image depuis un `Dockerfile` :**
  ```bash
  docker build -t <nom_image>:<tag> <chemin_du_dockerfile>
  ```
  *Exemple :* `docker build -t my_app:1.0 .`
- **Lister toutes les images :**
  ```bash
  docker images
  ```
- **Taguer une image :**
  ```bash
  docker tag <image_ID> <nom_du_repo>:<tag>
  ```
  *Exemple :* `docker tag my_image myrepo/my_image:1.1`

---

### 🏃 **Exécution et Gestion des Conteneurs**

- **Lancer un conteneur en mode interactif :**
  ```bash
  docker run -it <nom_image>
  ```
- **Exécuter un conteneur en arrière-plan (mode détaché) :**
  ```bash
  docker run -d <nom_image>
  ```
- **Exécuter une commande dans un conteneur actif :**
  ```bash
  docker exec -it <nom_du_conteneur_ou_ID> <commande>
  ```
  *Exemple :* `docker exec -it my_container /bin/bash`

---

### 🔧 **Volumes et Persistance de Données**

- **Créer un volume :**
  ```bash
  docker volume create <nom_du_volume>
  ```
- **Monter un volume dans un conteneur :**
  ```bash
  docker run -d -v <nom_du_volume>:<chemin_dans_le_conteneur> <nom_image>
  ```
  *Exemple :* `docker run -d -v my_volume:/app/data my_image`

---

### 🔍 **Gestion des Réseaux**

- **Lister les réseaux Docker :**
  ```bash
  docker network ls
  ```
- **Créer un réseau :**
  ```bash
  docker network create <nom_du_réseau>
  ```
- **Attacher un conteneur à un réseau :**
  ```bash
  docker network connect <nom_du_réseau> <nom_du_conteneur>
  ```

---

### 📋 **Compose et Automatisation (Docker Compose)**

- **Lancer tous les services dans un fichier `docker-compose.yml` :**
  ```bash
  docker-compose up -d
  ```
- **Arrêter tous les services d’un `docker-compose.yml` :**
  ```bash
  docker-compose down
  ```
- **Recréer un service spécifié :**
  ```bash
  docker-compose up -d --force-recreate <nom_du_service>
  ```

---

### ⚙️ **Volumes et Permissions pour Jenkins avec Docker**

- **Volumes pour Jenkins avec Docker Compose :**
  ```yaml
  services:
    jenkins:
      volumes:
        - /srv/data/jenkins_data:/var/jenkins_home
        - /srv/data/jenkins:/var/jenkins_config
      group_add:
        - $(getent group docker | cut -d: -f3)
  ```

---

### 🐳 **Nettoyage Docker**

- **Supprimer tous les conteneurs inactifs :**
  ```bash
  docker container prune
  ```
- **Supprimer toutes les images non utilisées :**
  ```bash
  docker image prune
  ```
- **Supprimer tous les volumes inutilisés :**
  ```bash
  docker volume prune
  ```

---

### 🎯 **Commandes d’Utilisation Avancée**

- **Gestion des permissions pour Docker :**
  ```bash
  sudo usermod -aG docker $USER
  ```
- **Utilisation de ZAP dans Docker :**
  ```bash
  docker run -d ghcr.io/zaproxy/zaproxy:stable
  ```
- **Utilisation avec Jenkins et DefectDojo :** voir configurations spécifiques dans les pipelines CI/CD.

---
