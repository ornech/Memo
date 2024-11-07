# APT (Advanced Package Tool)

#### Introduction
APT (Advanced Package Tool) est un gestionnaire de paquets utilisé principalement dans les distributions Linux basées sur Debian.

#### Commandes de Base

- **Mettre à jour la liste des paquets**
   ```sh
   sudo apt-get update
   ```
   Met à jour la liste des paquets depuis les distants
  > 🗒️ Notez
  > `apt update` emmet des requêtes dns pour atteindre les dépôts distants. Il faut donc impérativement configurer une adresse dns.

- **Mettre à niveau les paquets installés**
   ```sh
   sudo apt-get upgrade
   ```
   Met à jour tous les paquets installés si une version plus récente est disponible.

- **Mettre à niveau les paquets avec suppression des paquets obsolètes**
   ```sh
   sudo apt-get full-upgrade
   ```
   Met à jour le système en installant/mettant à jour les paquets

- **Installer un paquet**
   ```sh
   sudo apt-get install <nom_du_paquet>
   ```
   Remplacez `<nom_du_paquet>` par le nom du paquet que vous souhaitez installer.

- **Supprimer un paquet**
   ```sh
   sudo apt-get remove <nom_du_paquet>
   ```
   Supprime le paquet mais conserve les fichiers de configuration.

- **Supprimer un paquet et ses fichiers de configuration**
   ```sh
   sudo apt purge <nom_du_paquet>
   ```
   Supprime le paquet et tous ses fichiers de configuration.

- **Nettoyer les paquets inutilisés**
   ```sh
   sudo apt autoremove
   ```
   Supprime automatiquement les dépendances inutilisées

- **Nettoyer le cache des paquets**
   ```sh
   sudo apt clean
   ```
   Supprime les fichiers `.deb` téléchargés présent dans le cache (répertoire /var/cache/apt/archives/).

- **Rechercher un paquet**
   ```sh
   apt search <mot_clé>
   ```
   Recherche les paquets contenant le mot-clé spécifié.

- **Afficher des informations sur un paquet**
    ```sh
    apt show <nom_du_paquet>
    ```
    Affiche des informations disponibles sur le paquet spécifié.

#### Gestion des Dépôts

- **Ajouter un dépôt**
   ```sh
   add-apt-repository <dépôt>
   ```
   Remplacez `<dépôt>` par l'URL ou le nom du dépôt a ajouter.

- **Supprimer un dépôt**
   ```sh
   add-apt-repository --remove <dépôt>
   ```
   Remplacez `<dépôt>` par l'URL ou le nom du dépôt a supprimer.

- **Lister les dépôts**
   ```bash
   cat /etc/apt/sources.list
   cat /etc/apt/sources.list.d/*
   ```
   Affichent les dépôts configurés dans les fichiers `sources.list` et `sources.list.d`.

#### Options Utiles

- **Résoudre les problèmes de dépendances**
   ```bash
   apt --fix-broken install
   ```
   Permet de résoudre les problèmes de dépendances manquantes ou d'installation incomplète.

- **Forcer la réinstallation d'un paquet**
   ```bash
   sudo apt --reinstall install <nom_du_paquet>
   ```
   Réinstalle le paquet spécifié.

- **Bloquer la mise à jour automatique**
   ```bash
   apt-mark hold <nom_du_paquet>
   ```
   Empêche la mise à jour automatique d'un paquet lors d'un `apt upgrade`. Utilisez `apt-mark unhold <nom_du_paquet>` pour réactiver la mise à jour automatique d'un paquet.

- **Lister les paquets marqués "hold"**
   ```bash
   apt-mark showhold
   ```
---
Tableau récapitulatif des commandes liées à **APT** (Advanced Package Tool) sous Debian/Ubuntu, ainsi que de leurs fonctions :

| **Commande**             | **Description**                                                                                                    |
|--------------------------|--------------------------------------------------------------------------------------------------------------------|
| **`apt`**                 | Outil de gestion des paquets simplifié: installation, mise à jour, suppression, etc.             |
| **`apt-cdrom`**           | Permet d'ajouter un CD-ROM comme source de paquets pour `APT`. Utilisé pour les systèmes sans connexion Internet.   |
| **`aptdcon`**             | Outil graphique pour gérer les paquets en ligne de commande à partir de `APT`. C'est une interface pour `apt` en mode console. |
| **`apt-get`**             | Outil de gestion des paquets plus avancé que `apt`. Référez vous à la doc `man apt-get` |
| **`apt-sortpkgs`**        | Trie les paquets `.deb` téléchargés, ce qui peut être utile pour l'archivage ou la gestion des paquets localement.  |
| **`apt-add-repository`**  | Permet d'ajouter des dépôts supplémentaires à la liste de sources de `APT` .        |
| **`apt-config`**          | Utilitaire pour afficher ou modifier la configuration d'APT. Permet d'examiner les paramètres d'APT.                |
| **`apt-extracttemplates`**| Utilisé pour extraire des modèles de configuration à partir de paquets `.deb` pour les personnaliser.                |
| **`apt-key`**             | Outil pour gérer les clés GPG utilisées pour vérifier les paquets des dépôts.                                      |
| **`apt-cache`**           | Permet de manipuler le cache des paquets. Utilisé pour rechercher des paquets, afficher des informations sur les paquets, etc. |
| **`aptd`**                | Démon de gestion des paquets utilisé dans certaines interfaces graphiques comme `aptitude` ou `synaptic`.            |
| **`apt-ftparchive`**      | Génère des archives de paquets pour un dépôt local. Utilisé pour créer des dépôts de paquets à partir de fichiers `.deb`. |
| **`apt-mark`**            | Permet de marquer un paquet pour qu'il soit "en hold" (ne pas être mis à jour) ou pour d'autres manipulations d'état des paquets. |


  
