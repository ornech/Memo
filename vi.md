# MEMO Vi: Text Editor

## Utilisation basique
`:w` Sauvegarde  
`:x` Sauvegarde et quitte  
`:q`Quitte si aucun changement  
`:q! `Quitte sans sauvegarder  
`:set nu` Affiche les N° de ligne  
`:set nonu`Masque les N° de ligne  
`:undo`Annuler la dernière action  
`:redo`Refaire la dernière action  

## Mode insertion
`i` au curseur  
`I` En début de ligne   
`A` En fin de ligne  
`o` Nouvelle ligne après ligne courante   
`O` Nouvelle ligne avant ligne courante  
`ESC` Sort du mode insertion  

## Copier/Coller
`yy` Copie la ligne courante  
`:y` Copie la ligne courante  
`yw` Copie un mot  
`p` Coller après  
`P` Colle avant  

## Remplacement
`C` Remplace une ligne à partir de la position du curseur   
`cc` Remplace une ligne entière  

## Recherche
`/text` Recherche après le curseur  
`?text` Recherche avant le curseur  
`n` Résultat suivant  
`N` Résultat vers l’arrière   

## Remplacement
`:%s/aaa/bbb/g` Remplace toutes les chaînes de caractères aaa par la bbb  
`:%s/aaa/bbb/c` Même chose mais demande confirmation avant remplacement  

## Déplacement du curseur
`h` Recule d’un caractère  
`j` Descend  d’un caractère  
`k` Monte d’un caractère  
`l` Avance d’un caractère  
`w` début du prochain mot  
`b` début du mot précédent  
`e` fin du prochain mot  
`{` Paragraphe précédent  
`}` Paragraphe suivant  
`0` Début de la ligne  
`$` Fin de la ligne  
`5G` 5ème ligne  
`:3` 3éme ligne  
`H` Haut de la fenêtre   
`M` Milieu de la fenêtre   
`L` Bas de la fenêtre  
`G` Fin du fichier  

