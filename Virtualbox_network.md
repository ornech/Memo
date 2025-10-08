# Autoriser le NAT pour une VM 
Prérequis :
configuration réseau VM: "Réseau hôte privé" + reseau vboxnet configuré pour votre réseau de VM ( ici 192.168.56.0) 

### 1. Activer le routage IP sur l’hôte

```bash
# Active le forwarding IP (permet au noyau Linux de router les paquets entre interfaces)
sudo sysctl -w net.ipv4.ip_forward=1

# Vérification (1 = activé, 0 = désactivé)
cat /proc/sys/net/ipv4/ip_forward
```

👉 Pour le rendre permanent : ajoute dans `/etc/sysctl.conf` :

```
net.ipv4.ip_forward=1
```

---

### 2. Ajouter la règle NAT pour VirtualBox

```bash
# Masquerade (NAT) : tout le trafic issu du réseau 192.168.56.0/24 (VMs)
# sortira vers Internet via l’interface Wi-Fi de l’hôte (wlp0s20f3)
sudo iptables -t nat -A POSTROUTING -s 192.168.56.0/24 -o wlp0s20f3 -j MASQUERADE
```

Vérifier :

```bash
sudo iptables -t nat -L -n -v
```

---

### 3. Autoriser le forwarding du trafic

```bash
# Autoriser les VM (réseau 192.168.56.0/24) à sortir vers Internet
sudo iptables -A FORWARD -s 192.168.56.0/24 -o wlp0s20f3 -j ACCEPT

# Autoriser le trafic de retour (réponses aux connexions établies)
sudo iptables -A FORWARD -d 192.168.56.0/24 -m state --state ESTABLISHED,RELATED -i wlp0s20f3 -j ACCEPT
```

Vérifier :

```bash
sudo iptables -L FORWARD -n -v
```

---

### 4. Côté VM (Debian dans VirtualBox)

Vérifier que la passerelle est bien `192.168.56.1` (IP de l’hôte sur vboxnet0) :

```bash
ip route
```

Tu dois voir :

```
default via 192.168.56.1 dev enp0s3
```

---

### 5. Tests de connectivité

Depuis la VM :

```bash
# Tester la sortie Internet
ping -c 3 1.1.1.1

# Tester la résolution DNS
ping -c 3 debian.org
```
