# Mémo sur la commande `tar`

#### Introduction
`tar` (Tape Archive) est une commande utilisée pour compresser, archiver, et extraire des fichiers et répertoires sous Linux. Elle est couramment utilisée pour sauvegarder des fichiers ou les transférer en un seul fichier compressé.

#### Syntaxe de Base
La syntaxe de base de `tar` est la suivante :

```sh
tar [options] fichier_archive nom_fichiers_ou_dossiers
```

#### Options Principales
Voici les options les plus couramment utilisées avec `tar` :

1. **c** - Créer une nouvelle archive (`c` pour "create").
2. **x** - Extraire une archive existante (`x` pour "extract").
3. **t** - Lister le contenu d'une archive sans l'extraire (`t` pour "list").
4. **v** - Afficher des informations détaillées pendant l'exécution (`v` pour "verbose").
5. **f** - Spécifier le nom du fichier d'archive (`f` pour "file").
6. **z** - Compresser ou décompresser l'archive avec `gzip` (`z` pour "zip").
7. **j** - Compresser ou décompresser avec `bzip2` pour une meilleure compression (`j` pour "bzip2").
8. **J** - Compresser ou décompresser avec `xz` pour une compression maximale (`J` pour "xz").

#### Exemples de Commandes Courantes

1. **Créer une archive non compressée**
   ```sh
   tar -cvf archive.tar /chemin/du/dossier
   ```
   Crée une archive `archive.tar` contenant le dossier spécifié.

2. **Créer une archive compressée avec gzip**
   ```sh
   tar -czvf archive.tar.gz /chemin/du/dossier
   ```
   Crée une archive compressée `archive.tar.gz` avec gzip.

3. **Créer une archive compressée avec bzip2**
   ```sh
   tar -cjvf archive.tar.bz2 /chemin/du/dossier
   ```
   Crée une archive compressée `archive.tar.bz2` avec bzip2 (plus lente mais plus compacte).

4. **Créer une archive compressée avec xz**
   ```sh
   tar -cJvf archive.tar.xz /chemin/du/dossier
   ```
   Crée une archive compressée `archive.tar.xz` avec xz (plus compacte mais plus lente).

5. **Extraire une archive gzip**
   ```sh
   tar -xzvf archive.tar.gz
   ```
   Extrait l'archive `archive.tar.gz` dans le répertoire courant.

6. **Extraire une archive bzip2**
   ```sh
   tar -xjvf archive.tar.bz2
   ```
   Extrait l'archive `archive.tar.bz2` dans le répertoire courant.

7. **Extraire une archive xz**
   ```sh
   tar -xJvf archive.tar.xz
   ```
   Extrait l'archive `archive.tar.xz` dans le répertoire courant.

8. **Lister le contenu d'une archive**
   ```sh
   tar -tvf archive.tar.gz
   ```
   Affiche le contenu de l'archive `archive.tar.gz` sans l'extraire.

#### Options Supplémentaires

- **-C** : Spécifie le répertoire de destination pour l'extraction.
  ```sh
  tar -xzvf archive.tar.gz -C /chemin/de/destination
  ```
  Extrait l'archive dans le répertoire `/chemin/de/destination`.

- **--exclude** : Exclut des fichiers ou dossiers spécifiques lors de la création de l'archive.
  ```sh
  tar -czvf archive.tar.gz /chemin/du/dossier --exclude=/chemin/du/dossier/exclure_ceci
  ```

- **-u** : Met à jour une archive existante avec de nouveaux fichiers, sans recréer l'archive.
  ```sh
  tar -uvf archive.tar /nouveau/fichier
  ```

#### Utilisation des Canaux de Compression

Vous pouvez utiliser `tar` avec différents algorithmes de compression en fonction de vos besoins :
- **gzip** (option `-z`) : Rapide, mais avec une compression moyenne.
- **bzip2** (option `-j`) : Plus lente, mais une meilleure compression.
- **xz** (option `-J`) : La compression la plus élevée, mais aussi la plus lente.

#### Résumé des Commandes les Plus Utilisées

| Commande                           | Description                                        |
|------------------------------------|----------------------------------------------------|
| `tar -cvf archive.tar dossier`     | Crée une archive non compressée                    |
| `tar -czvf archive.tar.gz dossier` | Crée une archive compressée avec gzip              |
| `tar -cjvf archive.tar.bz2 dossier`| Crée une archive compressée avec bzip2             |
| `tar -cJvf archive.tar.xz dossier` | Crée une archive compressée avec xz                |
| `tar -xvf archive.tar`             | Extrait une archive non compressée                 |
| `tar -xzvf archive.tar.gz`         | Extrait une archive gzip                           |
| `tar -xjvf archive.tar.bz2`        | Extrait une archive bzip2                          |
| `tar -xJvf archive.tar.xz`         | Extrait une archive xz                             |
| `tar -tvf archive.tar.gz`          | Liste le contenu d'une archive gzip                |
