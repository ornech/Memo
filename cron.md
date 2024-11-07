# Mémo sur Cron

#### Introduction
Cron est un planificateur de tâches utilisé dans les systèmes Unix et Linux pour exécuter des commandes ou des scripts à des intervalles de temps spécifiés. Ce mémo couvre les bases de l'utilisation de Cron.

#### Syntaxe de Base
Un fichier crontab (cron table) contient des lignes de configuration pour les tâches planifiées. Chaque ligne suit le format suivant :

```
* * * * * commande
```

Les cinq champs représentent respectivement :
1. **Minute** (0-59) - Spécifie la minute exacte.
2. **Heure** (0-23) - Spécifie l'heure.
3. **Jour du mois** (1-31) - Spécifie le jour du mois.
4. **Mois** (1-12) - Spécifie le mois.
5. **Jour de la semaine** (0-7) - Spécifie le jour de la semaine (0 et 7 représentent le dimanche).

**Remarques** : 
- L’astérisque `*` signifie « chaque unité de temps » pour le champ correspondant.
- Vous pouvez utiliser `/` pour indiquer des intervalles : `*/5` signifie « toutes les 5 unités » (par exemple, toutes les 5 minutes).
  
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

#### Variables d'Environnement
Dans un fichier crontab, il est possible de définir des variables d'environnement. Par exemple, pour recevoir les notifications par mail en cas d’erreur :

```
MAILTO="user@example.com"
* * * * * echo "Hello, World!"
```

#### Astuces pour Cron
1. **Utiliser des chemins complets** : Indiquez les chemins absolus pour les scripts et commandes (ex. `/usr/bin/python3` ou `/home/user/monscript.sh`), car le chemin d’accès (`$PATH`) dans l’environnement Cron peut être différent de celui de votre utilisateur.
   
2. **Gestion des permissions** : Lorsqu'une tâche nécessite des privilèges administratifs, vérifiez si l'exécution doit se faire avec `sudo`, surtout pour les crontabs d'autres utilisateurs.

#### Redirection de la Sortie
Pour enregistrer les résultats et les erreurs d'une commande dans un fichier de log :

```
* * * * * echo "Hello, World!" >> /path/to/logfile.log 2>&1
```

#### Utilisation de Scripts
Vous pouvez exécuter des scripts shell en spécifiant le chemin complet du script :

```
* * * * * /path/to/script.sh
```

#### Ressources et Documentation
Pour des informations détaillées et des options avancées, consultez la documentation officielle de Cron :

- [Documentation de Cron sur Linux](https://man7.org/linux/man-pages/man5/crontab.5.html)
- `man crontab` pour consulter directement depuis le terminal.
