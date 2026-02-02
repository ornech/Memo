# TRIGGER

Observation :  
```sql
INSERT INTO table1 VALUES (...);

Effet observable : une ou plusieurs instructions SQL sont exécutées automatiquement lorsqu’un événement intervient sur une table (INSERT, UPDATE, DELETE).

Un trigger permet de :

imposer une règle métier,

automatiser des contrôles ou des tâches,

garantir l’intégrité des données (ex. stock négatif, durée négative, prix < 0).



---

BEFORE / AFTER

Observation :

UPDATE table1 SET col = 5 WHERE id = 1;

Un déclencheur est exécuté à un moment précis :

BEFORE : avant la modification de chaque ligne visée.

AFTER : après la modification de chaque ligne visée.


Conséquence directe :
le trigger est exécuté ligne par ligne, jamais sur l’ensemble du résultat.


---

OLD / NEW

Observation :

UPDATE table1 SET prix = 10 WHERE id = 1;

Pendant l’exécution du trigger :

OLD représente l’état précédent de la ligne.

NEW représente l’état courant (ou futur) de la ligne.


Ces deux pseudo-enregistrements ne sont valides que pendant l’exécution du trigger et uniquement pour une ligne à la fois.


---

FOR EACH ROW

Observation :

UPDATE table1 SET col = col + 1;

Le moteur InnoDB exécute le trigger séparément pour chaque ligne affectée afin de garantir la cohérence des valeurs OLD et NEW.

Séquence interne :

sélection d’une ligne,

préparation des valeurs OLD et NEW,

exécution du trigger,

passage à la ligne suivante.



---

Règle critique

Un trigger ne peut pas modifier la table qu’il surveille.
Violation → erreur 1442.

Risque : boucle infinie ou incohérence transactionnelle.


---

Syntaxe minimale

Observation :

UPDATE table1 SET date = NULL;

DELIMITER //

CREATE TRIGGER update_date
BEFORE UPDATE ON table1
FOR EACH ROW
SET NEW.date = NOW();

//

DELIMITER ;


Ce qu’un trigger permet réellement

Protéger une base de données contre des usages incorrects.

Déplacer les contrôles d’intégrité du code applicatif vers le SGBD.

Déclencher automatiquement des instructions SQL suite à un événement.


un trigger renforce l’intégrité mais rigidifie le schéma.
Un mauvais choix est difficile à corriger sans impacter les données existantes.