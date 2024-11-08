# Commande `ip` sous Linux

La commande `ip` est un outil puissant pour gérer et configurer les réseaux sous Linux. Elle fait partie du paquet **iproute2** et permet de visualiser, ajouter, supprimer, ou modifier des interfaces réseau, des adresses IP, et des routes. La commande `ip` est souvent préférée à des outils plus anciens comme `ifconfig` et `route` en raison de sa flexibilité et de ses fonctionnalités avancées.

## Gestion des adresses IP
#### Afficher la configuration réseau
```bash
ip a
```
Affiche toutes les interfaces réseau et leurs adresses IP (IPv4 et IPv6).

#### Attribuer une adresse IP
```bash
ip addr add 192.168.1.100/24 dev eth0
```
Attribue l’adresse IP `192.168.1.100` avec un masque de sous-réseau `/24` à l'interface `eth0`.

#### Supprimer une adresse IP
```bash
ip addr del 192.168.1.100/24 dev eth0
```
Supprime l’adresse IP `192.168.1.100` de l'interface `eth0`.

#### Supprimer toutes les adresses IP d'une interface
```bash
ip addr flush dev eth0
```

## Gestion des interfaces

#### Afficher les informations sur les interfaces réseau
```bash
ip link show
```
Affiche les informations détaillées sur toutes les interfaces réseau.

#### Changer l'état d'une interface (activer/désactiver)
```bash
ip link set eth0 up
ip link set eth0 down
```
Active (`up`) ou désactive (`down`) l'interface `eth0`.

## Gestion des routes
#### Afficher la table de routage
```bash
ip route
```
Montre la table de routage actuelle, y compris les routes par défaut et spécifiques.

#### Configurer une route
```bash
ip route add 192.168.2.0/24 via 192.168.1.1
```
Ajoute une route pour atteindre le réseau `192.168.2.0/24` via la passerelle `192.168.1.1`.


## Gestion des VLAN

#### Ajouter une interface VLAN
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

## Espace de nom réseau (Netwok namespace)
Un espace de noms réseau (ou network namespace) sous Linux est une fonctionnalité qui permet d'isoler les ressources réseau (interfaces, tables de routage, ports, etc.) pour des groupes de processus. Chaque espace de noms réseau possède sa propre pile réseau indépendante, ce qui signifie que les processus dans cet espace voient uniquement les interfaces réseau et configurations qui y sont associées.

 - Ajouter un espace de nom réseau
   ```bash
   ip netns add <nom>
   ```

 - Pour affichez vos espaces de nom réseau
   ```bash
   ip netns show
   ```

   Vous les trouverez également dans le dossier `/var/run/netns`.

 - Ajouter une interface réseau à un netspace  
   
    1) Création d'une interface virtuelle.
    
    ```bash
    ip link add <veth_1_1> type veth peer name <veth_1_2>
    ```
    > Les interfaces virtuelles fonctionnent par paire interconnectées (`veth_1_1` et `veth_1_2`) afin de former une sorte de tunnel entre des espaces de nom réseau. Ce qui entre d’un côté, ressort de l’autre. Pour plus d’informations: [lien](https://man7.org/linux/man-pages/man4/veth.4.html). 
    2) Associer une extrémité d'une interface virtuelle à un espace de nom réseau
    ```bash
    ip link add <veth_1_2> type veth peer name <netspace>
    ```

- Exécuter un processus dans un espace de nom réseau
  ```bash
  ip netns exec <netspace> <app>
  ```

  **exemple:** `ip netns exec netspace1 bash`, ici nous lancons le shell bash dans l'espace de nom réseau isolé `netspace1`, où toutes les commandes n'accéderont qu'aux interfaces et configurations réseau propres à cet espace de nom.
