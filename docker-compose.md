# Mémo sur Docker Compose

Docker Compose est un outil permettant de définir et gérer des applications multi-conteneurs Docker. Il utilise un fichier YAML (`docker-compose.yml`) pour configurer les services, réseaux, volumes, et autres paramètres nécessaires.

## Structure de Base d'un Fichier `docker-compose.yml`

Le fichier `docker-compose.yml` est au cœur de Docker Compose et doit inclure au minimum une version et une liste de services.

### Exemple de Fichier `docker-compose.yml`
```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
      POSTGRES_DB: example

volumes:
  db_data:  # Volume pour persister les données de la base de données
```

---

## Commandes de Base pour Docker Compose

### Démarrer et Arrêter les Services

- **Démarrer les services** :
  ```bash
  docker-compose up
  ```

- **Démarrer en arrière-plan** :
  ```bash
  docker-compose up -d
  ```

- **Arrêter les services** :
  ```bash
  docker-compose down
  ```

- **Arrêter et supprimer les volumes** (utile en développement pour éviter l'accumulation de données non persistées) :
  ```bash
  docker-compose down -v
  ```

### Gestion des Services Individuels

- **Arrêter un service spécifique** :
  ```bash
  docker-compose stop web
  ```

- **Redémarrer un service spécifique** :
  ```bash
  docker-compose restart db
  ```

### Affichage et Suivi des Logs

- **Afficher les logs de tous les services** :
  ```bash
  docker-compose logs
  ```

- **Suivre les logs en temps réel** :
  ```bash
  docker-compose logs -f
  ```

### Construction et Liste des Conteneurs

- **Construire ou reconstruire les images sans démarrer** :
  ```bash
  docker-compose build
  ```

- **Lister les services actifs** :
  ```bash
  docker-compose ps
  ```

- **Démarrer et reconstruire les images** (en cas de modification du Dockerfile) :
  ```bash
  docker-compose up --build
  ```

---

## Variables d'Environnement

Docker Compose permet d’utiliser un fichier `.env` pour stocker les variables d’environnement, ce qui facilite la gestion de la configuration.

### Exemple de `.env`
```bash
POSTGRES_USER=example
POSTGRES_PASSWORD=example
POSTGRES_DB=example
```

### Utilisation dans `docker-compose.yml`
```yaml
services:
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}
```

---

## Volumes

Les volumes permettent de persister les données pour qu’elles soient conservées même si le conteneur est supprimé.

```yaml
services:
  db:
    image: postgres:latest
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:  # Déclare le volume
```

> **Bonnes pratiques** : 
> - **Nommer les volumes** pour organiser les ressources Docker.
> - **Gérer les volumes** pour nettoyer les données obsolètes en développement.

---

## Réseaux

Docker Compose permet de créer des réseaux personnalisés pour contrôler la communication entre services.

### Exemples de Réseaux Personnalisés

- **Réseaux isolés** : On peut isoler les services `web` et `db` en les plaçant dans des réseaux différents (`frontend` et `backend`), limitant ainsi la communication inter-services pour plus de sécurité.

```yaml
services:
  web:
    image: nginx:latest
    networks:
      - frontend

  db:
    image: postgres:latest
    networks:
      - backend

networks:
  frontend:
    driver: bridge

  backend:
    driver: bridge
```

- **Sous-réseaux** : Définir un sous-réseau pour un contrôle IP précis.
  
```yaml
networks:
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.10.0/24
```

### Types de Pilotes Réseau

Docker prend en charge plusieurs pilotes :
- **Bridge** (par défaut) : utilisé pour des réseaux locaux isolés.
- **Host** : le conteneur partage le réseau du host (utile pour réduire la latence liée à la translation d'IP). 
- **Overlay** : connecte des conteneurs sur différents hôtes (Swarm).
- **Macvlan** : chaque conteneur a sa propre adresse MAC pour une intégration réseau avancée.

---

## Dépendances entre Services

L'option `depends_on` permet de gérer l’ordre de démarrage des services, bien que cela ne garantisse pas que le service soit prêt (pour une base de données, un délai supplémentaire peut être nécessaire).

```yaml
services:
  web:
    image: nginx:latest
    depends_on:
      - db

  db:
    image: postgres:latest
```

---

## Création d'Images Personnalisées

Pour des configurations spécifiques, on peut créer une image personnalisée via un Dockerfile. Utilisez `build` dans `docker-compose.yml` pour spécifier l’emplacement du Dockerfile.

```yaml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "80:80"
```

---

## Bonnes Pratiques pour la Production

- Utiliser des **secrets** pour les informations sensibles.
- **Éviter `latest`** : spécifier la version de l'image (par ex. `nginx:1.19`) assure la stabilité.
- Limiter les services aux **réseaux nécessaires** uniquement.

