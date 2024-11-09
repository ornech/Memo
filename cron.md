# Mémo sur Cron

#### Introduction
Cron est un planificateur de tâches utilisé dans les systèmes Unix et Linux pour exécuter des commandes ou des scripts à des intervalles de temps spécifiés. Ce mémo couvre les bases de l'utilisation de Cron.

```bash
# Fichier crontab

SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Exemple de planification de tache:
# .---------------- Minute (0 - 59)
# |  .------------- Heure (0 - 23)
# |  |  .---------- Jour du mois (1 - 31)
# |  |  |  .------- Mois (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- Jour de la semaine (0 - 6) oubien sun, mon, tue, wed ... 
# |  |  |  |  |
# *  *  *  *  * commande
```
#### Variables d'environnement
**$PATH :** La variable PATH définit le chemin par défaut où se trouve les commandes que vous souhaitez exécuter.

**$SHELL :** Définit le shell que cron devra utiliser pour exécuter vos tâches.

**$HOME :** La variable $HOME peut être définie dans crontab si vous avez besoin de mentionner un répertoire de travail spécifique.  

**$MAILTO :** Permet la notification par e-mail en cas d'erreur. Il est possible de mentionner liste de destinataire en séparant par des virgules les adresses.

#### Syntaxe de Base
Un fichier crontab (cron table) contient des lignes de configuration pour les tâches planifiées. Chaque ligne suit le format suivant :

```
* * * * * commande
```

Les cinq champs représentent respectivement :
| Position       | Paramètres       | Valeurs possibles                                                                 |
|----------------|------------------|-----------------------------------------------------------------------------------|
| 1er champs     | Les minutes      | 0-59                                                                              |
| 2ème champs    | Les heures       | 0-23                                                                              |
| 3ème champs    | Le jour du mois  | 0-31                                                                              |
| 4ème champs    | Le mois          | 1-12 ou jan, fev, mar, apr, may, jun, etc...                                       |
| 5ème champs    | Le jour de la semaine | 0-6 où 0=dimanche, sun, mon, tue, wed, thu, fri, sat                              |
| 6ème champs    | Commande         | Utilisez la syntaxe du SHELL que vous avez défini. Sont autorisés les pipelines et redirections. |


#### Symboles spéciaux dans les expressions cron
| Symbole  | Description                                                                 | Exemple                                                                 |
|----------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `*`      | Signifie « chaque unité de temps » pour le champ correspondant.             | `*` dans le champ des minutes signifie « chaque minute ».               |
| `/`      | Utilisé pour indiquer des intervalles.                                     | `*/5` dans le champ des minutes signifie « toutes les 5 minutes ».       |
| `[]`     | Utilisé pour spécifier une plage de valeurs.                               | `[0-5]` dans le champ des minutes signifie « de 0 à 5 minutes ».          |

#### Exemples de Crontab

1. **Toutes les minutes**
   ```
   * * * * * echo "Hello, World!"
   ```

2. **Toutes les heures**
   ```
   0 * * * * echo "Hello, World!"
   ```

3. **Tous les jours à 2h du matin**
   ```
   0 2 * * * echo "Hello, World!"
   ```

4. **Tous les dimanches à 3h du matin**
   ```
   0 3 * * 0 echo "Hello, World!"
   ```

5. **Tous les jours à 5h du matin et 5h du soir**
   ```
   0 5,17 * * * echo "Hello, World!"
   ```

6. **Toutes les 5 minutes**
   ```
   */5 * * * * echo "This runs every 5 minutes"
   ```

#### Éditer le Crontab
Pour éditer le crontab de l'utilisateur courant, utilisez :

```sh
crontab -e
```

Pour éditer le crontab d'un autre utilisateur :

```sh
sudo crontab -u nom_utilisateur -e
```

#### Lister les Tâches Cron
Pour lister les tâches cron de l'utilisateur courant :

```sh
crontab -l
```

Pour lister les tâches cron d'un autre utilisateur :

```sh
sudo crontab -u nom_utilisateur -l
```

#### Supprimer les Tâches Cron
Pour supprimer toutes les tâches cron de l'utilisateur courant :

```sh
crontab -r
```

Pour supprimer toutes les tâches cron d'un autre utilisateur :

```sh
sudo crontab -u nom_utilisateur -r
```

#### Astuces pour Cron
1. **Utiliser des chemins complets** : Indiquez les chemins absolus pour les scripts et commandes (ex. `/usr/bin/python3` ou `/home/user/monscript.sh`), car le chemin d’accès (`$PATH`) dans l’environnement Cron peut être différent de celui de votre utilisateur.
   
2. **Gestion des permissions** : Lorsqu'une tâche nécessite des privilèges administratifs, vérifiez si l'exécution doit se faire avec `sudo`, surtout pour les crontabs d'autres utilisateurs.

#### Redirection de la Sortie
Pour enregistrer les résultats et les erreurs d'une commande dans un fichier de log :

```
* * * * * echo "Hello, World!" >> /path/to/logfile.log 2>&1
```

#### Ressources et Documentation
Pour des informations détaillées et des options avancées, consultez la documentation officielle de Cron :

- [Documentation de Cron sur Linux](https://man7.org/linux/man-pages/man5/crontab.5.html)
- `man crontab` pour consulter directement depuis le terminal.
