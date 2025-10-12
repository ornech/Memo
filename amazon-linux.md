# Conf réseau
```bash
sudo nano /etc/systemd/network/10-cloud-init-eth0.network
```

Ajouter une IP fixe et gateway
```bash
[Match]
Name=eth0

[Network]
DHCP=yes
```
Vérification
```bash
[wazuh-user@wazuh-server ~]$ sudo resolvectl status
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: uplink

Link 2 (eth0)
Current Scopes: DNS
     Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
   DNS Servers: 8.8.8.8 1.1.1.1
[wazuh-user@wazuh-server ~]$ 

```
