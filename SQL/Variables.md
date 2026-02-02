# VARIABLES

## 1) Variable utilisateur (session)

Observation :  
```sql
SET @a = 10;
SELECT @a;

Portée : session uniquement.
Typage : faible → le type n’est pas fixé.
Préfixe : @ → ex. @val1.

Exemples d’affectation

SET @a = 10;          -- méthode 1 : SET (SQL standard)
SET @b := 20;         -- méthode 2 : SET avec :=
SELECT @c := @a + @b; -- méthode 3 : affectation dans un SELECT
SELECT 50 INTO @d;    -- méthode 4 : affectation par résultat de requête

SELECT @a, @b, @c, @d;

Exemple de typage faible

SET @val1 := 10;
SET @val2 := 'bleu';

DESCRIBE @val1;

SET @val1 := 'rouge';

DESCRIBE @val1;

Note critique :

En SQL, = est un opérateur de comparaison (sauf dans SET).

:= est un opérateur d’affectation explicite.

Règle d’or : utiliser systématiquement :=.


Constats vérifiables :

Une variable utilisateur n’a pas de type défini (untyped).

Le type dépend de la valeur courante, pas de la variable.

Les variables utilisateur sont propres à la session (connexion).

Elles sont détruites à la fin de la session.

Elles ne sont pas stockées dans le dictionnaire de données, mais en mémoire.




## VARIABLE LOCALE

Observation :

BEGIN
  DECLARE v INT;
  SET v = 10;
  SELECT v;
END;

Ces variables servent à construire des traitements robustes
(triggers, procédures stockées, fonctions).
Elles nécessitent d’autres mécanismes que les variables @.

1) Scope (portée)

La frontière d’une variable locale est strictement encadrée par :

BEGIN
  ...
END

En dehors du bloc, la variable n’existe plus.

2) Stack (pile)

Mémoire limitée et rigide (≈ 256 Ko par défaut).

Une pile par session.

Typage obligatoire → une erreur ici peut faire échouer le bloc.


3) Isolation

Une pile par session.

Un crash n’impacte pas les autres sessions.


Exemple complet

DELIMITER //

BEGIN NOT ATOMIC
  DECLARE var3 INT;   -- déclaration + type obligatoire
  SET var3 = 10;
  SELECT var3;
END//

DELIMITER ;

BEGIN n’est pas qu’un simple regroupement syntaxique.
Il engage une portée stricte, une allocation mémoire dédiée
et des contraintes fortes sur le typage.