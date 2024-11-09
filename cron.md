# Mémo sur Cron

## Introduction
Cron est un planificateur de tâches utilisé dans les systèmes Unix pour exécuter des commandes ou des scripts à des **intervalles** de temps spécifiés. Le démon `crontd` qui est en charge de ce processus.

> Notez qu'un interval de temps est un cocept bien différent d'une horaire. Un interval est cycle tandis qu'une horaire est ponctuelle. Ici nous parlons d'action à exécuter de manière cyclique.

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

#### Exemple d'un modèle de fichier crontab
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
#### Syntaxe de Base
Un fichier crontab peut contenir plusieurs lignes. Chaque ligne correspond à une tache planifiée.

```
* * * * * commande
```

## Variables d'environnement
**$PATH :** La variable PATH définit le chemin par défaut où se trouve les commandes que vous souhaitez exécuter.

**$SHELL :** Définit le shell que cron devra utiliser pour exécuter vos tâches.

**$HOME :** La variable $HOME peut être définie dans crontab si vous avez besoin de mentionner un répertoire de travail spécifique.  

**$MAILTO :** Permet la notification par e-mail en cas d'erreur. Il est possible de mentionner liste de destinataire en séparant par des virgules les adresses.

## Syntaxe détaullée

Les 6 champs représentent respectivement :
| Champs   | Paramètres       | Valeurs possibles                                                                 |
|----------------|------------------|-----------------------------------------------------------------------------------|
| 1er      | Les minutes      | 0-59                                                                              |
| 2ème    | Les heures       | 0-23                                                                              |
| 3ème    | Le jour du mois  | 0-31                                                                              |
| 4ème    | Le mois          | 1-12 ou jan, fev, mar, apr, may, jun, etc...                                       |
| 5ème    | Le jour de la semaine | 0-6 où 0=dimanche, sun, mon, tue, wed, thu, fri, sat                              |
| 6ème    | Commande         | Utilisez la syntaxe du SHELL que vous avez défini. Sont autorisés les pipelines et redirections. |


#### Symboles spéciaux dans les expressions cron
| Symbole  | Description                                                                 | Exemple                                                                 |
|----------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------|
| `*`      | Signifie "chaque unité de temps"             | `*` dans le champ des minutes signifie « chaque minute ».               |
| `/`      | Indique une intervalle.                                     | `*/5` dans le champ des minutes signifie « toutes les 5 minutes ».       |
| `[]`     | Spécifie une plage de valeurs.                               | `[0-5]` dans le champ des minutes signifie « de 0 à 5 minutes ».          |

## Exemples de Crontab
**Exécuter une commande toutes les 5 minutes**  
`*/5 * * * * /path/to/command`

**Exécuter une commande tous les jours à 2h du matin**  
`0 2 * * * /path/to/command`

**Exécuter une commande tous les lundis à 3h du matin**  
`0 3 * * 1 /path/to/command`

**Exécuter une commande le premier jour de chaque mois à minuit**  
`0 0 1 * * /path/to/command`

**Exécuter une commande tous les jours de la semaine (lundi à vendredi) à 18h**  
`0 18 * * 1-5 /path/to/command`

**Redirection de la Sortie**  
`* * * * * echo "Hello, World!" >> /path/to/logfile.log 2>&1`


## Ressources et Documentation
Pour des informations détaillées et des options avancées, consultez la documentation officielle de Cron :

- [Documentation de Cron sur Linux](https://man7.org/linux/man-pages/man5/crontab.5.html)
- `man crontab` pour consulter directement depuis le terminal.
