# Les Déclencheurs (Triggers)

Un trigger peut déclencher l’exécution d'une ou  plusieurs instructions SQL supplémentaires, automatiquement lorsqu’un événement (`INSERT`, `UPDATE`, `DELETE`) intervient sur une table.
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
OLD et NEW représentent respectivement l’état précédent et l’état courant d’une ligne unique pendant l’exécution du trigger.

|Contexte|Accès autorisé|
|---|---|
|INSERT|`NEW`|
|UPDATE|`OLD` et `NEW`|
|DELETE|`OLD`|

## 5. FOR EACH ROW 

MariaDB **exige** la clause `FOR EACH ROW`car le moteur MariaDB ne peut fournir des valeurs cohérentes de `OLD` et `NEW` que pour **une seule ligne à la fois**. Il faut donc mentionner explicitement l’exécution **ligne par ligne** avec `FOR EACH ROW`.

Le moteur SQL :
1. sélectionne une ligne,
2. prépare ses valeurs `OLD` / `NEW`,
3. exécute le trigger,
4. passe à la ligne suivante.

Conséquences observables :  
– un `UPDATE` sur 10 000 lignes déclenche 10 000 exécutions,  
– une règle métier apparemment simple peut devenir un goulet de performance,  
– l’ordre d’exécution dépend du moteur et du nombre de lignes impactées.

## 6. Règles:
Un trigger ne peut pas modifier la table qu’il surveille : toute tentative provoque une erreur (1442).

## 7. Exemple minimal

```sql
DELIMITER //

CREATE TRIGGER t_auto_date_produit
BEFORE UPDATE ON produits
FOR EACH ROW
BEGIN
    SET NEW.date_derniere_modif = NOW();
END//

DELIMITER ;
```

## 9. A retenir

Un trigger permet de:  
- protéger la base de données contre des usages incorrects,  
- transférer la gestion du contrôle d'intégrité du code au SGBD.
- déclencher automatiquement une ou  plusieurs instructions SQL suite à un évènement (INSERT, UPDATE, DELETE) sur une table
- Un trigger ne peut pas modifier la table qu’il surveille
