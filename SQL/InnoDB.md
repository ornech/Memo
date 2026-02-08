# InnoDB

On plonge littéralement dans le cœur de MariaDB et son moteur de stockage par défaut : **InnoDB**. L’idée, c’est de décortiquer son architecture interne pour vraiment comprendre ce qui le rend si performant et si robuste.

On va essayer de comprendre pourquoi InnoDB est "le" moteur de référence. Pourquoi InnoDB est le choix par défaut : voir une évidence.
Et on va tenter de suivre le trajet d’une donnée de sa modification en mémoire jusqu’à son enregistrement bien au chaud sur le disque.

### 1. InnoDB, qu'est ce que c'est ?

Il faut bien comprendre que InnoDB ce n’est pas juste une option comme une autre. C’est la fondation: le socle sur lequel la quasi‑totalité des installations MariaDB sont bâties. 

Mais comment InnoDB a‑t‑il fait pour s’imposer comme le choix n°1 ?
C’est parce que InnoDB est un peu le couteau suisse des moteurs des base de données. 

Il garantit que :
 - vos données sont traitées avec une fiabilité absolue, quoi qu’il arrive (crash, coupure, surcharge de travail). 
 - permet à une foule d'utilisateurs de travailler en même temps sans se marcher sur les pieds.

### 2. Le « Buffer pool »
Partons d'un exemple simple: un atelier. Dans un atelier on y trouve, des outils, des matériaux, des étagères, un établi ,  des ouvriers , etc ... Si on imagine InnoDB comme un atelier, alors son établi est le **Buffer pool** . A la manière d'un établi, le Buffer pool est un espace  de travail en mémoire. C'est là que où tout ce passe.

Mais attention : cet établi est une ressource que doivent se partager les ouvriers, c'est à dire les  transactions. Et bien sur, à la manière des ouvriers dans une atelier, ils ont tous tendance à vouloir bosser en même temps. 

Mais avant d'aller plus loin, on est en droit de se demander sur quoi bossent t-ils exactement ?  Et bien sur les "données": mais sur les données stockées le cache le Buffer pool, c'est à dire sur l'établi.

Reprenons, le buffer pool est un espace de travail en RAM où sont présentent les données... mais que se passe t-il si les données à charger sont trop volumineuse ? on ne pourra jamais tout charger en RAM ...

C'est là où InnoDB devient très intéressant. Il utilise une version maline de l’algorithme **LRU** (Least Recently Used). 
Le pool buffer est divisé en 2 parties :
- la « new list » pour les données chaudes: Ce sont les données que l'on manipule le plus souvent
- la "old list" qui sert de sas d’entrée. On y retrouve les données que l'on manipule pour la première fois.

**L'astuce  est la suivante:**  Pour qu'une donnée soit considérée comme "chaude" (new list), elle doit avoir été consultée deux fois. Ainsi, cela empêche qu'une opération ponctuelle volumineuse (comme l'affichage d'une table gigantesque) vienne expulser toutes les données importantes qui étaient bien au chaud. Notez que le temps passé dans la _Old List_ (variable `innodb_old_blocks_time`) est également pris en compte.

Retenez bien cela, toutes les opération sur les données sont réalisées en RAM.

> Cela nous emmène à la variable de configuration la plus importante : `innodb_buffer_pool_size`. Un bon réglage ce cette variable (en général 70 et 80% de la RAM) garantit  un espace mémoire suffisamment grand pour garder en mémoire toutes les données les plus souvent manipulée, rendant ainsi leur accès quasi instantanée.

### 3. Le début du voyage : que se passe‑t‑il quand on modifie une donnée
Avant toute **écriture persistante** d’une page issue du buffer pool, le moteur impose un filet de sécurité : le **WAL** (_Write-Ahead Logging_, soit une **journalisation avant écriture**).  
Toute tentative de modification d'une données doit être **journalisée dans un fichier sur le disque** avant qu'elle puisse être considérées comme validée.

InnoDB s’appuie sur **deux journaux distincts** : **REDO** et **UNDO**.

### Le journal REDO

Le journal REDO est un peu comme le proverbe **« chose promise, chose due »**.

Une fois qu’une modification est déclarée valide (`COMMIT`) dans le buffer pool, le moteur s’engage à écrire ces données sur disque , même si cela doit se faire plus tard.

Cela sert principalement en cas d’arrêt brutal du serveur : lors d’un crash, InnoDB relit le journal REDO et **rejoue les transactions validées mais pas encore écrites sur disque**.

C’est un des mécanismes qui garantit qu’aucune donnée validée ne sera perdue (**durabilité**).

### Le journal UNDO

Ce journal est peu le **rétroviseur** : on continue d’avancer, mais on garde toujours en vue ce qui a été fait.  Ceci se concrétise par la fait qu'avant de modifier une donnée "validée", InnoDB conserve de quoi reconstruire ce qui a été modifié.

Cela sert à deux choses :

1. Si une transaction échoue ou est annulée, InnoDB sera en mesure **revenir en arrière** (**ROLLBACK**).
2. Si une ligne est en cours de modification, les autres transactions qui souhaitent **consulter** (lire) cette ligne n'ont pas à attendre. InnoDB leur fournit la **version antérieure** de la ligne à partir du log UNDO. Ce mécanisme s'appelle un **snapshot read** et permet d'accéder aux données sans attendre la fin d'une transaction concurrente. C'est le cœur du **MVCC (Multi-Version Concurrency Control)**.

Le journal UNDO est donc un mécanisme **actif en permanence**, indispensable au rollback et à **l’isollation des transactions**.

**Comment InnoDB parvient à maintenir des performances élevées alors que des centaines d'utilisateurs tentent de modifier les mêmes tables simultanément ?**

Si le MVCC (accéder aux données sans attendre la fin d'une transaction concurrente) permet aux lecteurs de ne jamais être bloqués, la modification simultanée d'une même donnée par deux utilisateurs nécessite des mécanismes rigoureux pour éviter le chaos. InnoDB utilise pour cela le **Record Locking** (verrouillage de ligne).

1. Le Verrouillage par ligne (Row-Level Locking): InnoDB ne verrouille que la ligne concernée contrairement à MyISAM qui bloque toute une table.
2. Le principe du "Fail Fast" (Timeout): Si une transaction est mise en attente trop longtemps (50s par défaut), elle est interrompue (variable `innodb_lock_wait_timeout`). 
3. Verrouillage d'Index: InnoDB verrouille uniquement l'entrée correspondante à un **index** d'une table. C'est ce qui permet l'accès simultanée à une table par d'autre transaction.
4. La détection de Deadlock (Le boucle infini):  Quand deux transactions se bloquent mutuellement (A  qui attend B et B qui attend A), InnoDB interrompt la transaction la plus légère pour débloquer la situation.

> [!NOTE] Les Index : Le pilier de la performance
> C'est le point le plus important à retenir pour un administrateur de base de données. 
>  1) Avec un index, InnoDB sait exactement quelle ligne verrouiller.
>  2) Sans index (sur un `UPDATE` avec un `WHERE` mal optimisé par exemple), InnoDB est obligé de parcourir toute la table et **toutes les lignes examinées** seront verrouillées.
>  3) L'index n'est pas seulement là pour lire plus vite (SELECT), il est indispensable pour **écrire sans bloquer les autres transations**.


> [!warning] Le piège de l'absence d'index
L'index n'est pas une simple option d'optimisation de vos tables pour les requêtes `SELECT`. 
Sans index, InnoDB travaille "en l'aveugle" :
> - Au lieu de verrouiller une seule ligne, InnoDB devra verrouiller **toute la table** bloquant  tout le monde.
>- Si des milliers de lignes doivent être parcouru pour trouver la ligne à modifier, le moteur saturera inutilement le **Buffer Pool** et les journaux (**Undo Log**).


> [!notes] Règle d'or
>   Une modification (`UPDATE` / `DELETE`) sans index est une erreur de conception gravisime qui peut potentiellement faire tomber un serveur en production.

### 4. « Changement permanent »

Nos modifications sont maintenant sécurisées grâce aux logs. Il est temps maintenant de les graver dans le marbre sur le disque. Mais attention, InnoDB ne travaille pas par petits morceaux : il manipule des **Pages**.

C’est l’unité de base, un bloc de **16 Ko**. Imaginez que votre base de données est un grand livre : InnoDB ne lit et n'écrit jamais un seul mot, il déplace toujours une page entière.

C'est ici qu'apparaît la **« page sale » (Dirty page)** : c'est une page que vous avez modifiée en mémoire (dans la RAM) et qui n'est donc plus identique à celle qui est sur le disque. Elle est « sale » parce qu'elle est en attente de synchronisation.

Cette synchronisation, le **page flushing**, est gérée en arrière-plan par des travailleurs de l’ombre : les **page cleaners**. Ils parcourent la mémoire et écrivent tranquillement les pages modifiées sur le disque pour ne pas perturber les opérations en cours. Ils font le ménage sans que l'utilisateur ne s'en aperçoive.

**La ceinture de sécurité :** Juste avant l'écriture finale, InnoDB fait une copie rapide dans le **Doublewrite Buffer**. Pourquoi ? Parce qu'écrire 16 Ko sur un disque prend un petit instant. Si le courant coupe _pendant_ que le disque écrit, votre page peut se retrouver « déchirée » (moitié ancienne, moitié nouvelle) et donc corrompue.

Grâce au Doublewrite Buffer, on a toujours une copie propre sous la main pour réparer les dégâts au redémarrage. C’est le garant ultime de la **Cohérence**.

### 5. L’équilibre des forces

Pour finir, InnoDB est un équilibre parfait entre trois forces :

– Le **Buffer pool** pour la vitesse (travail en mémoire).
– Les journaux **REDO** et le **Doublewrite** pour la sécurité (durabilité).
– Le journal **UNDO** et le **MVCC** pour la fluidité (isolation).

C’est cet équilibre qui répond au défi de l'ingénierie : offrir le meilleur des deux mondes, la vitesse et la sécurité. Ça laisse à réfléchir à tous ces compromis qui se cachent au cœur de la technologie qu’on utilise tous les jours sans forcément y penser.

# Bilan

|**Étape**|**Lieu**|**Action**|**État de la donnée (Physique)**|**État de la transaction (Logique)**|
|---|---|---|---|---|
|**1. Lecture**|Disque → Buffer Pool|Chargement en RAM|**Propre**|N/A|
|**2. Modif**|Buffer Pool|Modification en RAM|**Dirty (Sale)**|En cours|
|**3. Sécurité**|Log Buffer → REDO Log|Écriture du log sur disque|**Dirty**|**Validée (Committed)**|
|**4. Checkpoint**|Buffer Pool → Disque|Synchronisation finale|**Propre**|Archivée|
