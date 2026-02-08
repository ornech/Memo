# Transaction SQL

Est un contexte d’exécution dans lequel plusieurs instructions SQL sont liées par une promesse du moteur (pas par la syntaxe).

→ Tant que la transaction n’est pas validée, **RIEN** n’est définitivement écrit.

Une transaction impose 4 contraintes (**ACID**).
---
## 1. Atomicité
`COMMIT` applique tout.  
`ROLLBACK` annule tout.  
Il n’existe pas d’état intermédiaire valide.

---
## 2. Cohérence
Les contraintes (TYPE, PRIMARY KEY, UNIQUE, FOREIGN KEY, NOT NULL, DEFAULT, CHECK, TRIGGER) doivent être respectées **avant la fin** de la transaction.

---

## 3. Isolation
Une transaction ne voit jamais les changements non validés (`COMMIT`) des autres transactions en cours ou concurrentes.

---

## 4. Durabilité
Après un `COMMIT`, les effets sont permanents, même après un plantage système.

---

## Synthèse
Une transaction ACID est une promesse du moteur (innoDB) :

- Tout ou rien (**Atomicité**)
- Respect des règles d’intégrité (**Cohérence**)
- Pas d’interférences entres session (**Isolation**)
- Effets persistants après validation (**Durabilité**)

---
# Suite (2) 
## TP — Démonstration ACID

### Préparation  
→ 2 sessions ouvertes
- Session A ; Console
- Session B : Dbeaver 

```sql
CREATE TABLE test_acid (
  id INT PRIMARY KEY,
  label VARCHAR(50)
) ENGINE=InnoDB;

INSERT INTO test_acid VALUES (1, "initial");
```
---
## SYNTAXE

#### Transaction Validée (COMMIT)

```sql
START TRANSACTION;
	...
COMMIT; -- si les règles ACID sont respectées
```


#### Transaction annulée (ROLLBACK)
```sql
START TRANSACTION;
	...
ROLLBACK; -- Annule toutes les requêtes précédentes
```

---

## 1) Atomicité
### Session A (Console)
Nous allons réaliser une transaction qui fait : 
- UPDATE sur la table test_acid
- attendra 30 s avant de valider la transaction.


```sql
START TRANSACTION;
UPDATE test_acid SET label = "Modifié" WHERE id = 1;
SELECT * FROM test_acid;
SELECT SLEEP(30);
COMMIT;
```

---
## 1) Atomicité
### Session B (Dbeaver)
Avant la fin des 30s, fermer la session A.
```sql
SELECT * FROM test_acid;
```

> **Observation :**  
>La valeur ne change pas car la promesse du moteur d’atomicité (« tout ou rien ») n’a pas pu être remplie.  Un `ROLLBACK` est exécuté automatiquement.

---

## 2) Cohérence ( respect des règles)

### Session A (console)
```sql
START TRANSACTION;
INSERT INTO test_acid VALUES (2, "Premier passage");
SELECT * FROM test_acid;
```

Provoquer une erreur de cohérence :

```sql
INSERT INTO test_acid VALUES (2, "Doublon");
```

Erreur 1062 → la clé n°2 existe déjà (doublon).

```sql
ROLLBACK;
SELECT * FROM test_acid;
```

**Observation :**  
ROLLBACK annule l’ensemble des requêtes exécutées précédemment pour conserver une base saine.

---

## 3) Isolation

### Session A (console)

```sql
START TRANSACTION;
INSERT INTO test_acid VALUES (2, "Brouillon A");
```

Réponse : `Query OK`
```sql
SELECT - FROM test_acid; -- l'insertion est présente
```

```sql
SELECT CONNECTION_ID(); -- affiche le n° de session
-- n° 1665 pour moi
```

---

### Session B (DBeaver)

```sql
SELECT * FROM test_acid;
```

DBeaver ne voit rien.

**Observation :**
- Ce n'est pas un bug
- Le moteur InnoDB respecte la règle d’isolation.
- DBeaver a ouvert une autre session.

```sql
SELECT CONNECTION_ID(); -- le n° de session est diffrent
-- n° 1666
```
---

### session A

```sql
COMMIT;
```

### Session B

```sql
SELECT * FROM test_acid;
```

Une fois validée depuis la session A, la requête `INSERT` est définitivement écrite dans la table. Ceci valide de principe d'isolation.

Et confirme également le point D, à savoir la **durabilité**.

---

# Suite — Transactions SQL

## Tableau des effets (ACID)

### A — Atomicité

`BEGIN NOT ATOMIC`  
Chaque instruction est validée dès sa réussite.  
Le stock est disponible pour le script sans les contraintes de journalisation.

---

### C — Cohérence
#### Bloquer une régle
```SQL
FOREIGN_KEY_CHECK = 0
``` 
Le moteur ne vérifie plus les index des tables parentes.  
Indispensable pour les imports massifs.

---

### I — Isolation

`READ UNCOMMITTED`  
Le thread ignore les verrous posés par les autres sessions.  
Accès en RAM aux autres threads sans attendre leur `COMMIT`.

---

### D — Durabilité

`InnoDB_flush_log = 0`  
Le journal InnoDB n’est pas immédiatement persisté sur disque.
