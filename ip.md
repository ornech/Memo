# Mémo sur la commande `ip` sous Linux

La commande `ip` est un outil puissant pour gérer et configurer les réseaux sous Linux. Elle fait partie du paquet **iproute2** et permet de visualiser, ajouter, supprimer, ou modifier des interfaces réseau, des adresses IP, et des routes. La commande `ip` est souvent préférée à des outils plus anciens comme `ifconfig` et `route` en raison de sa flexibilité et de ses fonctionnalités avancées.

### Afficher la configuration réseau
```bash
ip a
```
Affiche toutes les interfaces réseau et leurs adresses IP (IPv4 et IPv6).

### Attribuer une adresse IP
```bash
ip addr add 192.168.1.100/24 dev eth0
```
Attribue l’adresse IP `192.168.1.100` avec un masque de sous-réseau `/24` à l'interface `eth0`.

### Supprimer une adresse IP
```bash
ip addr del 192.168.1.100/24 dev eth0
```
Supprime l’adresse IP `192.168.1.100` de l'interface `eth0`.

### Changer l'état d'une interface (activer/désactiver)
```bash
ip link set eth0 up
ip link set eth0 down
```
Active (`up`) ou désactive (`down`) l'interface `eth0`.

### Configurer une route
```bash
ip route add 192.168.2.0/24 via 192.168.1.1
```
Ajoute une route pour atteindre le réseau `192.168.2.0/24` via la passerelle `192.168.1.1`.

### Afficher la table de routage
```bash
ip route
```
Montre la table de routage actuelle, y compris les routes par défaut et spécifiques.

### Afficher les informations sur les interfaces réseau
```bash
ip link show
```
Affiche les informations détaillées sur toutes les interfaces réseau.

### Supprimer toutes les adresses IP d'une interface
```bash
ip addr flush dev eth0
```
---

## Gestion des VLAN

### Ajouter une interface VLAN
```bash
ip link add link eth0 name eth0.10 type vlan id 10
```
Ajoute une interface VLAN `eth0.10` avec l'ID VLAN `10` sur l'interface `eth0`.

#### Supprimer une interface VLAN
```bash
ip link delete eth0.10
```
Supprime l'interface VLAN `eth0.10`.

#### Activer une interface VLAN
```bash
ip link set eth0.10 up
```
Active l'interface VLAN `eth0.10`.

#### Désactiver une interface VLAN
```bash
ip link set eth0.10 down
```
Désactive l'interface VLAN `eth0.10`.
