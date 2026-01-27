# Les Déclencheurs (Triggers)

Un TRIGGER peut déclencher l’exécution automatiquement d'une ou  plusieurs instructions SQL suite à un `INSERT`, `UPDATE`ou `DELETE` sur une table.
Cela permet de:  
- imposer une règle métier **indépendamment du code applicatif**,  
- automatiser des mises à jour techniques
- garantir l'intégrité des données 

Ici, garantir l’intégrité des données signifie empêcher qu’une donnée incohérente soit enregistrée, même si la requête SQL a un syntaxe valide. Par exemple :
- un stock ne peut pas être négatif
- une durée ne peut pas être négative,  
- un prix ne peut pas être inférieur à zéro.

## 3. Moment d’exécution : BEFORE / AFTER 
Un déclencheur peut être exécuté à un moment précis suite à un évènement:
- **BEFORE** : exécuté avant la modification de **chaque ligne visée** par l’instruction.
- **AFTER** : exécuté après la modification de **chaque ligne visée** par l’instruction.

Conséquence directe :  
- `NEW` est modifiable uniquement en `BEFORE`,
- `AFTER` sert à réagir à une modification déjà actée, pas à la corriger.

## 4. Objets de transition : OLD et NEW
Les objets `OLD` et `NEW` donnent accès aux données avant et après modification pour chaque ligne concernée par l’événement.

|Contexte|Objet(s) disponible(s)|Remarque|
|---|---|---|
|INSERT|`NEW`| Il n'y a pas d'ancienne valeur lors d'un INSERT |
|UPDATE|`OLD` et `NEW`| - |
|DELETE|`OLD`| Il n'y a pas de nouvelle valeur suite à une DELETE |

## 5. FOR EACH ROW 
MariaDB impose la clause FOR EACH ROW car une instruction déclenchante peut affecter plusieurs lignes, alors que les objets OLD et NEW ne représentent qu’une ligne unique. Le moteur exécute donc le trigger séparément pour chaque ligne affecté par le TRIGGER afin de garantir des valeurs cohérentes de OLD et NEW.

Le moteur SQL :
1. sélectionne une ligne,
2. prépare ses valeurs `OLD` / `NEW`,
3. exécute le trigger,
4. passe à la ligne suivante.

Conséquences :  
– Si un TRIGGER sur un `UPDATE` d'un table impact 10 000 lignes, le TRIGGER exécutera 10 000 fois la tache qu'il est censé faire.  


## 6. Règles:
- MariaDB interdit un `INSERT`, `UPDATE`, `DELETE` sur la **même table** que celle qui a déclenché le trigger (Erreur 1442).
- Un trigger peut exécuter des requêtes sur **d’autres tables** sans enfreindre cette restriction.
- Les objets `NEW` et `OLD` permettent d’accéder/ajuster **les valeurs de la ligne en cours** sans exécuter une requête SQL sur une table surveillé par le TRIGGER.

## 7. Exemple minimal

```sql
CREATE TRIGGER update_date
BEFORE UPDATE ON table1
FOR EACH ROW
SET NEW.date = NOW();

```

## 9. A retenir

Un trigger permet de:  
- protéger la base de données contre des usages incorrects,  
- transférer la gestion du contrôle d'intégrité du code au SGBD.
- déclencher automatiquement une ou  plusieurs instructions SQL suite à un évènement (INSERT, UPDATE, DELETE) sur une table
- Un trigger ne peut pas modifier la table qu’il surveille
