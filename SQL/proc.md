# PROCÉDURE STOCKÉE

Une procédure stockée est une suite d’instructions SQL stockée dans le SGBD que l’on peut exécuter sur un simple appel.

---

## Structure

```sql
CREATE PROCEDURE nom (param1 TYPE, param2 TYPE)
BEGIN
  -- instructions
END;

```

⚠️ Le changement de DELIMITER est nécessaire pour distinguer la fin de la procédure des requetes de la procedure.

# Paramètres (IN, OUT, INOUT)

IN : valeur fournie lors de l’appel de la procédure (lecture seule).

OUT : désigne un paramètre de sortie, une variable où la procédure écrit le résultat.

INOUT : valeur fournie lors de l’appel, puis modifiée par la procédure.


---

## Exemple paramètre IN

```
CREATE TEMPORARY TABLE tmp_table (
  multiplicateur INT,
  facteur INT,
  resultat INT
);

DELIMITER //

CREATE PROCEDURE multi (IN table INT)
BEGIN
  DECLARE i INT DEFAULT 1;

  WHILE i <= 10 DO
    INSERT INTO tmp_table (table, i, table * i);
    SET i = i + 1;
  END WHILE;

  SELECT * FROM tmp_table;
END//

DELIMITER ;

CALL multi(2);

```

## Exemple paramètre OUT


``` SQL
DELIMITER //

DROP PROCEDURE IF EXISTS calcul_carre//

CREATE PROCEDURE calcul_carre (IN nbr INT, OUT resultat INT)
BEGIN
  SET resultat = nbr * nbr;
END//

DELIMITER ;

```

Appel

```SQL
SET @r = NULL;
CALL calcul_carre(7, @r);
SELECT @r;
```
Notes :
Le paramètre OUT `resultat` a une portée locale,c’est a dire uniquement dans la procédure.

La portée de la variable `@r` se limite  à la session SQL de l’utilisateur.


---

## Exemple paramètre INOUT
``` SQL
DELIMITER //

DROP PROCEDURE IF EXISTS incrementer//

CREATE PROCEDURE incrementer (
  INOUT val INT
)
BEGIN
  SET val = val + 1;
END//

DELIMITER ;
```

Appel
```
SET @v = 10;
CALL incrementer(@v);
SELECT @v;
```

Notes :le paramètre INOUT est lu en entrée et réécrit en sortie.