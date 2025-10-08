Ajouter un DNS fonctionnel via systemd-resolved

```bash
sudo resolvectl dns enp0s3 8.8.8.8 1.1.1.1
sudo resolvectl domain enp0s3 "~."
```
Vérification;  

```bash
resolvectl status
```
