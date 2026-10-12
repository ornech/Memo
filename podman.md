# PODMAN
Système de conteneurisation developpé par REDHAT. Avantages:
- Syntaxe proche de Docker
- Pas de deamon central exécuté en root

## Installation
```shell
sudo apt update
sudo apt install podman
```
# IMAGES
Source docker
```shell
podman pull docker://docker.io/alpine:latest
```
Afficher les images 'utilisateur" disponibles
```shell
podman images
REPOSITORY                TAG         IMAGE ID      CREATED      SIZE
docker.io/library/alpine  latest      320994c3b997  3 weeks ago  8.72 MB
```
Afficher les images 'root" disponibles
```shell
sudo podman images
```
