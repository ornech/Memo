# **Git**


## **Configuration initiale**
1. **Configurer votre identité utilisateur** :
   ```bash
   git config --global user.name "Votre Nom"
   git config --global user.email "votre.email@example.com"
   ```

2. **Vérifier la configuration** :
   ```bash
   git config --list
   ```


## **Démarrer un projet Git**
1. **Initialiser un dépôt Git** :
   ```bash
   git init
   ```
    > La commande `git init` initialise le suivi de version dans le répertoire courant. Un répertoire caché .git sera créé, il servira a stocker toutes les informations de suivi de version pour de ce répertoire.

2. **Cloner un dépôt distant** :
   ```bash
   git clone <url-du-dépôt>
   ```


## **Gestion des identifiants (credentials)** 
Permettent de stocker les identifiants et token nécessaires à la connexion à un dépôt distant (par exemple GitHub).

1. **Git Credential Manager (`manager`)** 💯 :  
   - Stocke de manière **sécurisée et permanente** les identifiants dans le gestionnaire de mots de passe de ton système (macOS Keychain, Windows Credential Manager, ou Linux Keyring).
     ```bash
     git config --global credential.helper manager
     ```

2. **Cache temporaire (`cache`)** ⚠️:  
   - Stocke les identifiants **en mémoire** pour une durée limitée (par défaut 15 minutes, configurable).
     ```bash
     git config --global credential.helper cache
     ```
   - Stoker dans le cache les identifiants et le token pendant 1 heure (soit 3600s).
     ```bash
     git config --global credential.helper "cache --timeout=3600"
     ```
   
3. **Stockage en clair (`store`)** ⛔:  
   - Stocke les identifiants ⛔ **en clair** ⛔ dans un fichier texte (`~/.git-credentials`).
   ```bash
     git config --global credential.helper store`
   ```
> ℹ️ **Comment enregistrer mes identifiants et tokens**
>  - Exécutez une commande Git nécessitant une authentification (par exemple : `git pull origin).
>  - Saisissez votre identifiant et votre token lorsqu’ils sont demandés.
>  - Vos informations seront enregistrées dès cette première connexion, à condition d’avoir correctement configuré l’option credential.helper via :
git config --global credential.helper <option>
>  - Relancez ensuite une commande nécessitant une authentification : aucune demande d’identification ne devrait apparaître.


## **Gestion des fichiers**
1. **Vérifier l'état des fichiers** (modifications locales, fichiers suivis ou non suivis) :
   ```bash
   git status
   ```

2. **Ajoute un fichier à la staging area** :
   - Ajouter un fichier spécifique :
     ```bash
     git add <fichier>
     ```
   - Ajouter tous les fichiers :
     ```bash
     git add .
     ```
     > 🗒️ 1) Lorsque vous exécutez `git add`, vous indiquez à Git que le fichier est prêt à être ajouter à la prochaine validation (commit). Cela signifie que toutes les modifications (ajouts, suppressions, modifications) effectuées sur ce fichier seront inclus dans le prochain commit.
     
     > 🗒️ 2) seul les fichiers mentionnés dans la stagging area sont intégrés à la prochaine validation (commit).

3. **Validation des changements (commit)** :
   ```bash
   git commit -m "Message de commit"
   ```
   > Un commit dans Git est une photo instantanée de l'état de votre projet à un instant "t". C'est une opération clé dans l'historique d'un dépôt Git, car chaque commit représente un enregistrement permanent et versionné des changements apportés au code ou aux fichiers du projet.

4. **Annuler un ajout avant le commit** (réinitialiser un fichier à son état précédent) :
   ```bash
   git reset <fichier>
   ```


## **Branches**
1. **Lister les branches locales** :
   ```bash
   git branch
   ```

2. **Créer une nouvelle branche** :
   ```bash
   git branch <nom-de-branche>
   ```

3. **Changer de branche** :
   ```bash
   git checkout <nom-de-branche>
   ```

4. **Créer et changer de branche en une seule commande** :
   ```bash
   git checkout -b <nom-de-branche>
   ```

5. **Fusionner une branche dans la branche actuelle** :
   ```bash
   git merge <nom-de-branche>
   ```

6. **Supprimer une branche** :
   - Branches locales :
     ```bash
     git branch -d <nom-de-branche>
     ```
   - Branches distantes :
     ```bash
     git push origin --delete <nom-de-branche>
     ```


## **Travailler avec un dépôt distant**
1. **Ajouter un dépôt distant** :
   ```bash
   git remote add origin <url-du-dépôt>
   ```
   > `origin` : Est un alias par défaut désignant le dépôt distant principal.
   > Vous pouvez avoir plusieurs dépôts distants, mais origin sera celui utilisé par défaut lors du clonage ou de l'ajout initial.
 

2. **Vérifier les dépôts distants** :
   ```bash
   git remote -v
   ```

3. **Pousser (push) des commits locaux vers le dépôt distant** :
   ```bash
   git push origin <nom-de-branche>
   ```

4. **Tirer (pull) les modifications d'un dépôt distant** :
   ```bash
   git pull origin <nom-de-branche>
   ```

5. **Récupérer les changements sans les fusionner (fetch)** :
   ```bash
   git fetch origin
   ```
5. **[JOKER] Remplacer les fichiers locaux par les fichiers du dépôt distant** :
   ```bash
   git restore .
   git pull
   ```

## **Gestion des modifications**
- **Afficher les différences non validées** (entre le fichier modifié et la stagging area) :
   ```bash
   git diff
   ```

- **Afficher les différences entre deux commits** :
   ```bash
   git diff <commit1> <commit2>
   ```

- **Afficher l’historique des commits** :
   ```bash
   git log
   ```
   - Affichage succinct :
     ```bash
     git log --oneline
     ```

- **Annuler un commit local avant qu’il ne soit poussé** (réinitialiser le HEAD) :
   ```bash
   git reset --hard <commit-id>
   ```

- **Modifier le dernier commit (ajouter des modifications ou modifier le message)** :
   ```bash
   git commit --amend
   ```
   
- **Stasher (mettre de côté) des changements non validés** (utile avant de changer de branche) :
   ```bash
   git stash
   ```

- **Récupérer des changements précédemment mis de côté** :
   ```bash
   git stash pop
   ```

## **Autres commandes utiles**
1. **Afficher l'historique des modifications pour un fichier spécifique** :
   ```bash
   git log <fichier>
   ```

2. **Désactive le suivi de version pour un fichier** (sans le supprimer du disque) :
   ```bash
   git rm --cached <fichier>
   ```

3. **Restaure les fichiers tels qu'ils étaientt au dernier commit** (annuler toutes les modifications) :
   ```bash
   git checkout -- <fichier>
   ```

4. **💯 Afficher l’historique des commits sous forme de graphe** :
   ```bash
   git log --graph --oneline --all
   ```


## **Gestion des tags**
1. **Lister les tags** :
   ```bash
   git tag
   ```

2. **Créer un tag** :
   ```bash
   git tag <nom-du-tag>
   ```

3. **Pousser un tag vers un dépôt distant** :
   ```bash
   git push origin <nom-du-tag>
   ```

4. **Supprimer un tag localement** :
   ```bash
   git tag -d <nom-du-tag>
   ```

5. **Supprimer un tag distant** :
   ```bash
   git push origin --delete <nom-du-tag>
   ```


## Workflow git
Un workflow Git est la façon dont une équipe utilise Git pour organiser et gérer son code, les branches et les contributions. Il consiste en une série d'étapes standardisées que les contributeurs suivent tous afin de garantir une collaboration efficace, d'éviter les conflits de code et d'assurer une gestion fluide des versions.

Voci un exemple:  
→ **git pull**  
→ **git checkout -a nouvelle_branche**  
→ **Modification et ajouts de fichiers**  
→ **git add .**  
→ **git commit -M "Descritption de ce que j'ai modifié"**  
→ **git push**



## **Notes supplémentaires**
- **`git pull`** : Cette commande est équivalente à `git fetch` suivi de `git merge`. Elle permet de récupérer les changements distants et de les fusionner avec la branche actuelle.
- **`git reset --hard`** : Attention ! Cette commande supprime définitivement les changements non validés et ne peut pas être annulée.
- **Réécrire l'historique** (mode "Rebase") :
La commande `git rebase` est une méthode avancée de gestion des branches qui permet de réécrire l’historique des commits. `git rebase` prend les modifications de votre branche courante et les place au-dessus des derniers commites d’une autre branche (comme main), pour que votre historique soit plus propre, sans commits de fusion. (Notez c'est le moyen le plus rapide pour vous faire des ennemis à vie... )
   ```bash
   git rebase <branche-à-fusionner>
   ```

- **Résoudre les conflits de fusion** :
   - Modifier les fichiers conflictuels manuellement.
   - Marquer les conflits comme résolus :
     ```bash
     git add <fichier-conflit>
     ```
