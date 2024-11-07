# **Git**

### **Configuration initiale**
1. **Configurer votre identité utilisateur** :
   ```bash
   git config --global user.name "Votre Nom"
   git config --global user.email "votre.email@example.com"
   ```

2. **Vérifier la configuration** :
   ```bash
   git config --list
   ```

---

### **Démarrer un projet Git**
1. **Initialiser un dépôt Git** :
   ```bash
   git init
   ```

2. **Cloner un dépôt distant** :
   ```bash
   git clone <url-du-dépôt>
   ```

---

### **Travailler avec les fichiers**
1. **Vérifier l'état des fichiers** (modifications locales, fichiers suivis ou non suivis) :
   ```bash
   git status
   ```

2. **Ajouter des fichiers à l'index** (préparation à l'engagement) :
   - Ajouter un fichier spécifique :
     ```bash
     git add <fichier>
     ```
   - Ajouter tous les fichiers modifiés :
     ```bash
     git add .
     ```

3. **Engager (commit) les changements** :
   ```bash
   git commit -m "Message de commit"
   ```

4. **Annuler un ajout avant le commit** (réinitialiser un fichier à son état précédent) :
   ```bash
   git reset <fichier>
   ```

---

### **Branches**
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

---

### **Travailler avec un dépôt distant**
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

---

### **Gestion des modifications**
1. **Afficher les différences non validées** (entre le fichier modifié et l'index) :
   ```bash
   git diff
   ```

2. **Afficher les différences entre deux commits** :
   ```bash
   git diff <commit1> <commit2>
   ```

3. **Afficher l’historique des commits** :
   ```bash
   git log
   ```
   - Affichage succinct :
     ```bash
     git log --oneline
     ```

4. **Annuler un commit local avant qu’il ne soit poussé** (réinitialiser le HEAD) :
   ```bash
   git reset --hard <commit-id>
   ```

5. **Modifier le dernier commit (ajouter des modifications ou modifier le message)** :
   ```bash
   git commit --amend
   ```

---

### **Travail collaboratif**
1. **Stasher (mettre de côté) des changements non validés** (utile avant de changer de branche) :
   ```bash
   git stash
   ```

2. **Récupérer des changements précédemment mis de côté** :
   ```bash
   git stash pop
   ```

3. **Fusionner les branches sans enregistrer immédiatement** (mode "Rebase") :
   ```bash
   git rebase <branche-à-fusionner>
   ```

4. **Résoudre les conflits de fusion** :
   - Modifier les fichiers conflictuels manuellement.
   - Marquer les conflits comme résolus :
     ```bash
     git add <fichier-conflit>
     ```

---

### **Autres commandes utiles**
1. **Afficher l'historique des modifications pour un fichier spécifique** :
   ```bash
   git log <fichier>
   ```

2. **Supprimer un fichier du contrôle de version** (sans le supprimer du disque) :
   ```bash
   git rm --cached <fichier>
   ```

3. **Réinitialiser les fichiers à l’état du dernier commit** (annuler toutes les modifications locales) :
   ```bash
   git checkout -- <fichier>
   ```

4. **💯 Afficher l’historique des commits sous forme de graphe** :
   ```bash
   git log --graph --oneline --all
   ```

---

### **Gestion des tags**
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

---

### **Notes supplémentaires**
- **`git pull`** : Cette commande est équivalente à `git fetch` suivi de `git merge`. Elle permet de récupérer les changements distants et de les fusionner avec la branche actuelle.
- **`git reset --hard`** : Attention ! Cette commande supprime définitivement les changements non validés et ne peut pas être annulée.
