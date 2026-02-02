# PROCÉDURE STOCKÉE

Une procédure stockée est une suite d’instructions SQL stockée dans le SGBD que l’on peut exécuter sur un simple appel.

---

## Structure

```sql
CREATE PROCEDURE nom (param1 TYPE, param2 TYPE)
BEGIN
  -- instructions
END;

⚠️ Le changement de DELIMITER est nécessaire pour distinguer la fin de la procédure.
; n’encadre pas le bloc.


---

Paramètres (IN, OUT, INOUT)

IN : valeur fournie à l’appel de la procédure (lecture seule).

OUT : désigne un paramètre de sortie, une variable où la procédure écrit le résultat.

INOUT : valeur fournie à l’appel, puis modifiée par la procédure.



---

Exemple

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


---

Exemple avec OUT

DELIMITER //

DROP PROCEDURE IF EXISTS calcul_carre//

CREATE PROCEDURE calcul_carre (IN nbr INT, OUT resultat INT)
BEGIN
  SET resultat = nbr * nbr;
END//

DELIMITER ;

Appel

SET @r = NULL;
CALL calcul_carre(7, @r);
SELECT @r;

Notes :

resultat (paramètre OUT) a une portée locale, uniquement dans la procédure.

@r a une portée de session : elle existe uniquement en dehors de la procédure, dans la session SQL de l’utilisateur.



---

Exemple INOUT

DELIMITER //

DROP PROCEDURE IF EXISTS incrementer//

CREATE PROCEDURE incrementer (
  INOUT val INT
)
BEGIN
  SET val = val + 1;
END//

DELIMITER ;

Appel

SET @v = 10;
CALL incrementer(@v);
SELECT @v;

Notes :

INOUT est lu en entrée et réécrit en sortie.

La procédure lit la variable.

Ajoute 1.

Écrit dans cette même variable la nouvelle valeur, qui est aussi le paramètre de sortie.