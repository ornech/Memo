# APT (Advanced Package Tool)

#### Introduction
APT (Advanced Package Tool) est un gestionnaire de paquets utilisé principalement dans les distributions Linux basées sur Debian.

#### Commandes de Base

- **Mettre à jour la liste des paquets**
   ```sh
   sudo apt update
   ```
   Met à jour la liste des paquets depuis les distants
  > 🗒️ Notez
  > `apt update` emmet des requêtes dns pour atteindre les dépôts distants. Il faut donc impérativement configurer une adresse dns.

- **Mettre à niveau les paquets installés**
   ```sh
   sudo apt upgrade
   ```
   Met à jour tous les paquets installés si une version plus récente est disponible.

- **Mettre à niveau les paquets avec suppression des paquets obsolètes**
   ```sh
   sudo apt full-upgrade
   ```
   Met à jour le système en installant/mettant à jour les paquets

- **Installer un paquet**
   ```sh
   sudo apt install <nom_du_paquet>
   ```
   Remplacez `<nom_du_paquet>` par le nom du paquet que vous souhaitez installer.

- **Supprimer un paquet**
   ```sh
   sudo apt remove <nom_du_paquet>
   ```
   Supprime le paquet mais conserve les fichiers de configuration.

- **Supprimer un paquet et ses fichiers de configuration**
   ```sh
   sudo apt purge <nom_du_paquet>
   ```
   Supprime le paquet et tous ses fichiers de configuration.

7. **Nettoyer les paquets inutilisés**
   ```sh
   sudo apt autoremove
   ```
   Cette commande supprime les paquets qui ont été installés automatiquement pour satisfaire les dépendances d'autres paquets et qui ne sont plus nécessaires.

8. **Nettoyer le cache des paquets**
   ```sh
   sudo apt clean
   ```
   Cette commande supprime les fichiers de paquets téléchargés dans le cache.

9. **Rechercher un paquet**
   ```sh
   apt search <mot_clé>
   ```
   Cette commande recherche les paquets contenant le mot-clé spécifié.

10. **Afficher des informations sur un paquet**
    ```sh
    apt show <nom_du_paquet>
    ```
    Cette commande affiche des informations détaillées sur le paquet spécifié.

#### Gestion des Dépôts

1. **Ajouter un dépôt**
   ```sh
   sudo add-apt-repository <dépôt>
   ```
   Remplacez `<dépôt>` par l'URL ou le nom du dépôt que vous souhaitez ajouter.

2. **Supprimer un dépôt**
   ```sh
   sudo add-apt-repository --remove <dépôt>
   ```
   Remplacez `<dépôt>` par l'URL ou le nom du dépôt que vous souhaitez supprimer.

3. **Lister les dépôts**
   ```sh
   cat /etc/apt/sources.list
   cat /etc/apt/sources.list.d/*
   ```
   Ces commandes affichent les dépôts configurés dans les fichiers `sources.list` et `sources.list.d`.

#### Options Utiles

1. **Simuler une commande sans l'exécuter**
   ```sh
   sudo apt -s <commande>
   ```
   Remplacez `<commande>` par la commande que vous souhaitez simuler (par exemple, `install`, `upgrade`, etc.).

2. **Forcer la réinstallation d'un paquet**
   ```sh
   sudo apt --reinstall install <nom_du_paquet>
   ```
   Cette commande réinstalle le paquet spécifié.

3. **Afficher les paquets obsolètes**
   ```sh
   apt list --upgradable
   ```
   Cette commande affiche les paquets qui peuvent être mis à niveau.
