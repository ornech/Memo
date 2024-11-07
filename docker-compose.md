# Mémo sur Docker Compose
Docker Compose est un outil qui permet de définir et de gérer des applications multi-containers Docker. 

### Fichier docker-compose.yml

Ce fichier de configuration principal pour Docker Compose. Voici un exemple de base :
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
```
---
### Commandes de Base

 - **Démarrer les services**
   ```bash
   docker-compose up
   ```

- **Démarrer les services en arrière-plan**
  ```bash
  docker-compose up -d
  ```

- **Arrêter les services**
  ```bash
  docker-compose down
  ```

- **Afficher les logs**
  ```bash
  docker-compose logs
   ```

- **Redémarrer les services**
  ```bash
docker-compose restart
```

- **Construire les images**
```bash
docker-compose build
```
- **Lister les services**
- ```bash
  docker-compose ps
  ```

### Variables d'Environnement

Vous pouvez utiliser un fichier .env pour définir des variables d'environnement. Par exemple :
```bash
POSTGRES_USER=example
POSTGRES_PASSWORD=example
POSTGRES_DB=example
```
Et dans votre docker-compose.yml :
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
### Volumes

Les volumes permettent de persister les données entre les redémarrages des conteneurs.
```yaml
services:
  db:
    image: postgres:latest
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```
---
### Réseaux

Vous pouvez définir des réseaux personnalisés pour isoler les services.
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
  my_network:
```
