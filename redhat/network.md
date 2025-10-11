## Vérifier l’interface réseau active

```bash
ip route
192.168.56.0/24 dev eth0 proto kernel scope link src 192.168.56.7
```

Ici l’interface active est `eth0`.

## Ajouter une route par défaut **temporairement**

Commande immédiate :

```bash
sudo ip route add default via 192.168.56.1 dev eth0
```
Vérifiez

```bash
ip route
default via 192.168.56.1 dev eth0
192.168.56.0/24 dev eth0 proto kernel scope link src 192.168.56.7
```

##  Ajouter une route **persistante (méthode Red Hat)**

Sur les systèmes Red Hat-like utilisant **NetworkManager**, chaque interface a un fichier :

```
/etc/sysconfig/network-scripts/ifcfg-eth0
```

(ou `/etc/NetworkManager/system-connections/eth0.nmconnection` sur RHEL 9+)

### 👉 Méthode legacy (RHEL 7–8)

Éditez le fichier correspondant à ton interface, par exemple :

```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-eth0
```

Ajoutez ou vérifie les lignes suivantes :

```
DEFROUTE=yes
GATEWAY=192.168.56.1
```

Puis redémarrez le réseau :

```bash
sudo systemctl restart network
```

# Vérification

```bash
ip route show
ping -c 3 8.8.8.8
ping -c 3 google.com
```
