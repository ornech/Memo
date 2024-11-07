# Mémo sur Docker Compose

Docker Compose est un outil permettant de définir et gérer des applications multi-conteneurs Docker. Il utilise un fichier YAML (`docker-compose.yml`) pour configurer les services, réseaux, volumes, et autres paramètres nécessaires.

## Structure de Base d'un Fichier `docker-compose.yml`

Le fichier `docker-compose.yml` est au cœur de Docker Compose et doit inclure au minimum une version et une liste de services.

### Exemple de Fichier `docker-compose.yml`
```yaml
version: '3.8'  # Version de la syntaxe Docker Compose

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"  # Expose le port 80 à l'extérieur

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

- **Arrêter avec suppression des volumes** (utile en développement pour éviter l'accumulation de données non persistées) :
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

- **Démarrer les services en reconstruisant les images (en cas de modification du Dockerfile)** :
  ```bash
  docker-compose up --build
  ```

---

## Variables d'Environnement

Pour une meilleure gestion de la configuration, surtout en production, Docker Compose permet d’utiliser un fichier `.env` pour stocker les variables d’environnement.

### Exemple de `.env`
```bash
POSTGRES_USER=example
POSTGRES_PASSWORD=example
POSTGRES_DB=example
```

### Référence des Variables d’Environnement dans `docker-compose.yml`
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

Les volumes permettent de persister les données, afin qu’elles soient conservées même si le conteneur est supprimé. Cela est essentiel pour les bases de données.

### Exemple de Volume
```yaml
services:
  db:
    image: postgres:latest
    volumes:
      - db_data:/var/lib/postgresql/data  # Monte un volume pour la persistance des données

volumes:
  db_data:  # Déclare le volume
```

---

## Réseaux

Docker Compose permet de créer des réseaux personnalisés pour contrôler la communication entre services. Par exemple, un réseau isolé `my_network` peut être défini pour isoler les services `web` et `db`.

### Exemple de Réseau
```yaml
services:
  web:
    image: nginx:latest
    networks:
      - my_network

  db:
    image: postgres:latest
    networks:
      - my_network

networks:
  my_network:  # Déclare le réseau
```

### Isolés les réseaux des services
On peut isoler les services `web` et `db` en les plaçant dans des réseaux différents (par exemple le service `web` dans un réseau `frontend` et `db` dans le réseau `backend`). Cela améliore la sécurité en limitant les communications entre services à ce qui est nécessaire.

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - frontend

  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
      POSTGRES_DB: example
    networks:
      - backend

networks:
  frontend:
    driver: bridge  # Utilise le pilote bridge par défaut

  backend:
    driver: bridge  # Utilise également un réseau bridge isolé pour la base de données
```

Dans cet exemple, les services `web` et `db` sont isolés l'un de l'autre. Le conteneur `web` ne pourra pas accéder à `db` à moins d'être connecté au même réseau.

#### Utilisation d'un Sous-Réseau Personnalisé

Pour un contrôle réseau encore plus précis, vous pouvez définir des sous-réseaux spécifiques (par exemple, pour des besoins de routage ou de contrôle IP).

```yaml
networks:
  backend:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.10.0/24  # Définit un sous-réseau spécifique
```

Ici tous les conteneurs connectés au réseau `backend` auront des adresses IP dans la plage `192.168.10.0/24`, ce qui permet de gérer les connexions par IP fixe si nécessaire.

> Docker prend en charge plusieurs pilotes de réseau :
> - **Bridge** (par défaut) : utilisé pour des réseaux locaux isolés.
> - **Host** : le conteneur partage le réseau du host, utile pour éviter la latence liée à la translation d'adresses IP. Disponible uniquement sur Linux.
>   - **Overlay** : pour connecter des conteneurs sur différents hôtes dans un cluster Swarm.
>  - **Macvlan** : permet aux conteneurs d'avoir leur propre adresse MAC, utile pour l'intégration avec des réseaux physiques.

Exemple d'utilisation du pilote `host` (principalement pour des tests de performance) :

```yaml
networks:
  host_network:
    driver: host
```

#### Exemples de Réseau Multi-Services avec Accès Contrôlé

On peut combiner les réseaux pour permettre à certains services d'accéder à plusieurs réseaux. Par exemple, un service `api` peut accéder à `web` et `db` si les réseaux sont bien configurés.

```yaml
services:
  web:
    image: nginx:latest
    networks:
      - frontend

  api:
    image: myapi:latest
    networks:
      - frontend
      - backend

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
`api` peut accéder à `web` et `db`, mais `web` et `db` ne peuvent pas communiquer entre eux directement.

---

## Dépendances entre Services

L'option `depends_on` permet de s'assurer qu’un service démarre après un autre, bien que cela ne garantisse pas que le service soit totalement prêt (par exemple, une base de données peut prendre du temps pour être opérationnelle).

### Exemple d’Utilisation de `depends_on`
```yaml
services:
  web:
    image: nginx:latest
    depends_on:
      - db  # Le service `web` attend que `db` démarre

  db:
    image: postgres:latest
```

---

## Exemples Avancés

### Exemple d'Image Personnalisée avec Dockerfile

Pour des configurations spécifiques, on peut créer une image personnalisée en ajoutant un Dockerfile dans le projet. Utilisez l'option `build` dans `docker-compose.yml` pour spécifier l’emplacement du Dockerfile.

```yaml
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile  # Utilise le Dockerfile dans le répertoire courant
    ports:
      - "80:80"
```

### Commandes pour Environnements de Production

Pour des configurations de production, ajoutez les meilleures pratiques suivantes :
- Utiliser des **secrets** pour des informations sensibles comme des mots de passe.
- Toujours spécifier la version de l'image (`nginx:1.19` au lieu de `nginx:latest`) pour assurer la stabilité.

---

## Quelques Bonnes Pratiques

- **Nommer les volumes et réseaux** pour mieux organiser vos ressources Docker.
- **Éviter `latest`** : les versions explicites d'images assurent la stabilité (exemple : `nginx:1.19`).
- **Utiliser des secrets** en production pour la sécurité.

Avec cette structure et ces commandes, vous devriez être en mesure de créer, gérer, et configurer efficacement des environnements multi-conteneurs avec Docker Compose.
