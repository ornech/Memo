# DELIMITER

Observation :  
```sql
SELECT 1;
```

Le caractère ; marque la fin d’une instruction SQL.


Rôle du DELIMITER

Permet de changer temporairement le caractère indiquand l’exécution d’une instruction. Est nécessaire pour la creation de trigger ou procedure stockée.

Délimiteur par défaut `;`


Observation :

BEGIN
  SET a = 1;
  SET b = 2;
END;

Par défaut, le SGBD interprète chaque ; comme une fin de commande.
Résultat : un bloc est coupé avant d’être entièrement lu.

Ce problème apparaît notamment avec :
 - IF
 - CASE
 - WHILE
 - FOR


---

Syntaxe

Observation :

DELIMITER //

Effet :
// devient le nouveau marqueur de fin d’instruction.
Le code peut contenir librement des ;.

DELIMITER //

BEGIN
  SET a = 1;
  SET b = 2;
END//

DELIMITER ;

Retour à l’état normal :

DELIMITER ;


---

Règles implicites (mais vérifiables)

DELIMITER est une instruction côté client, pas SQL standard.

Il n’existe pas dans le moteur, uniquement dans l’outil d’exécution.

Oublier de rétablir ; provoque des erreurs difficiles à diagnostiquer.


le délimiteur ne protège pas la logique du code,
il empêche seulement le client de couper trop tôt.