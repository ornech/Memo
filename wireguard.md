# MEMO WIREGUARD
https://www.wireguard.com

## 🏗️ 1. Architecture du Tunnel
Pour que ça marche, les clés doivent être **croisées**.



| Machine | Sa Clé Privée | Sa Clé Publique |
| :--- | :--- | :--- |
| **Serveur** | `Priv_S` | `Pub_S` |
| **VM (Client)** | `Priv_V` | `Pub_V` |

---

## 📝 2. Les fichiers de Conf

### Sur le SERVEUR (`/etc/wireguard/wg0.conf`)
```ini
[Interface]
PrivateKey = <Priv_S>
Address = 10.100.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <Pub_V>           # Clé publique de la VM
AllowedIPs = 10.100.0.2/32    # IP autorisée pour la VM
```

### Sur la VM (`/etc/wireguard/wg0.conf`)
```ini
[Interface]
PrivateKey = <Priv_V>
Address = 10.100.0.2/24

[Peer]
PublicKey = <Pub_S>           # Clé publique du SERVEUR
Endpoint = <IP_LAN_SERVEUR>:51820
AllowedIPs = 10.100.0.1/32    # IP du serveur à joindre
```

---

## ⚡ 3. Commandes de Survie

### Gérer l'interface (Le plus simple)
*   **Relancer proprement :** `sudo wg-quick down wg0 && sudo wg-quick up wg0`
*   **Voir l'état :** `sudo wg show` (Cherche la ligne **handshake**).

### Extraire une clé publique
Si tu as perdu la clé publique d'un fichier :
```bash
sudo grep PrivateKey /etc/wireguard/wg0.conf | awk '{print $3}' | wg pubkey
```

---

## 🔍 4. Pourquoi ça bloque ? (Le 30 secondes check)
1.  **Handshake absent ?** → Mauvaise **PublicKey** dans le `[Peer]` opposé ou **Port UDP 51820** fermé.
2.  **Handshake OK mais pas de Ping ?** → L'IP dans `Address` ne correspond pas au `AllowedIPs` déclaré chez l'autre.
3.  **Erreur "Line unrecognized" ?** → Tu as utilisé `wg syncconf` au lieu de `wg-quick`. Utilise toujours `wg-quick`.
