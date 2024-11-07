#  DOCKER

### 🔧 **Problêmes divers**
- **Installation**
 https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

- **Gestion des permissions pour Docker :**
  Si erreur `dial unix /var/run/docker.sock: connect: permission denied`
  ```bash
  sudo usermod -aG docker $USER
   ```  
- **Afficher les logs d'un conteneur  :**
  ```bash
  docker logs <nom_conteneur>
   ```
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
  exemple: `docker build -t mon_image:latest /chemin/vers/le/dockerfile`
- **Lister toutes les images :**
  ```bash
  docker image ls
  ```
- **Taguer une image :**
  ```bash
  docker tag <image_ID> <nom_du_repo>:<tag>
  ```
  *Exemple :* `docker tag my_image myrepo/my_image:1.1`

- **Supprimer une image :**
  ```bash
  docker rmi <ID_de_l_image>
  ```
---

### 🐳 **Exécution et Gestion des Conteneurs**

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

### 💾 **Volumes et Persistance de Données**

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

### 🌍 **Gestion des Réseaux**

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
  > ℹ️ **Réseaux Docker par défaut**  
  >  * `bridge` : Réseau par défaut pour les conteneurs avec isolation  
  >  * `host` : Les conteneurs partagent directement le réseau de l'hôte, sans isolation  
  >  * `none` : Aucune configuration réseau (pas d'accès réseau)  
  
- **Mapper un port:** 
  ```bash
  docker run -d -p <port_local>:<port_du_conteneur> nginx
  ```
- **Créer un réseau utilisateur avec sous-réseau :**
   ```bash
   docker network create --subnet=10.10.36.0/24 <nom_du_reseau>
   ```

- **Attribuer une IP fixe lors du lancement du conteneur :**
   ```bash
   docker run -d --name <nom_du_conteneur> --network <nom_du_reseau> --ip 10.10.36.122 <nom_image>
   ```

- **Attribuer une IP fixe à un conteneur existant :**
   ```bash
   docker network connect --ip 10.10.36.122 <nom_du_reseau> <nom_du_conteneur>
---

### 🧹 **Nettoyage Docker**

- **Supprime un conteneur:**
  ```bash
  docker rm <nom_du_conteneur ou ID>
  ```  
-  ⚠️ **Supprimer tous les conteneurs arrêtés** (sans avoir besoin de spécifier leur nom ou ID):
  ```bash
  docker container prune
  ```
- ⚠️ **Supprimer toutes les images non utilisées :**
  ```bash
  docker image prune
  ```
- ⚠️ **Supprimer tous les volumes inutilisés :**
  ```bash
  docker volume prune
  ```

---

## Tableau récapitulatif des options utiles

Voici un tableau des options couramment utilisées et intéressantes avec `docker run`, qui vous aideront dans différentes situations :

| Option           | Description                                                                                                      | Exemple                                                                                           |
|------------------|------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `-d`             | Exécute le conteneur en mode détaché (en arrière-plan).                                                          | `docker run -d <nom_image>`                                                                       |
| `-it`            | Ouvre un terminal interactif dans le conteneur. Utile pour des sessions de commande.                            | `docker run -it <nom_image> /bin/bash`                                                            |
| `--name`         | Attribue un nom au conteneur, facilitant son identification.                                                     | `docker run --name mon_conteneur <nom_image>`                                                     |
| `-p`             | Redirige les ports entre l'hôte et le conteneur, permettant l'accès aux services du conteneur depuis l'extérieur.| `docker run -p 8080:80 <nom_image>`                                                               |
| `-v`             | Monte un volume entre l'hôte et le conteneur pour persister des données ou partager des fichiers.                | `docker run -v /chemin/local:/chemin/conteneur <nom_image>`                                       |
| `--rm`           | Supprime automatiquement le conteneur une fois arrêté, idéal pour des conteneurs temporaires.                   | `docker run --rm <nom_image>`                                                                     |
| `--env` ou `-e`  | Définit une variable d’environnement dans le conteneur, utile pour configurer l'application.                    | `docker run -e "VAR1=value1" <nom_image>`                                                         |
| `--network`      | Associe le conteneur à un réseau Docker spécifique pour gérer la communication avec d'autres conteneurs.         | `docker run --network mon_reseau <nom_image>`                                                     |
| `--cpus`         | Limite le nombre de processeurs utilisés par le conteneur, utile pour la gestion des ressources.                 | `docker run --cpus="1.5" <nom_image>`                                                             |
| `--memory` ou `-m` | Limite la mémoire disponible pour le conteneur pour éviter qu'il consomme trop de RAM.                       | `docker run -m 512m <nom_image>`                                                                  |
| `--restart`      | Définit une politique de redémarrage automatique pour le conteneur (utile pour les services).                    | `docker run --restart always <nom_image>`                                                         |
| `--log-driver`   | Configure le driver de journalisation pour le conteneur (json-file, syslog, etc.).                               | `docker run --log-driver syslog <nom_image>`                                                      |
| `--link`         | Connecte directement deux conteneurs, permettant à l'un d'accéder à l'autre par son nom (moins utilisé).         | `docker run --link conteneur1:alias_conteneur1 <nom_image>`                                       |
