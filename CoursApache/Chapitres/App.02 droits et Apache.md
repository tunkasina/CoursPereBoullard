### Appendice 2: droits Apache
#### Rappel Droits Unix
Je sais que vous vous en souvenez, mais je ne sais pas si on vous l'a expliqué correctement. Les droits Unix (vous savez, **640**, **777**, etc.) ce ne sont que des bits à **0** ou **1** qui autorisent des choses sur un fichier ou un dossier.

Chaque nombre représente un ou plusieurs droits, vous le savez. mais _pourquoi_ ? Qu'est ce qui fait que `rwx`=`7` ?

C'est bien plus simple qu'il n'y parait, si vous savez compter de tête en **binaire** de 1 à 7 vous pouvez faire la conversion de tête sans effort.

Les flags **r**, **w**, **x** sont des bits, qui sont soit à **0**, soit à **1**. Ainsi, si vous voyez affiché `rwx` par exemple, c'est comme si vous lisiez le nombre binaire `111`.  Et si vous voyez `rw-`et bien c'est `110`. Et `r--`, c'est `100`, en binaire.

Et enfin, il y a 3x3 bits, chaque paquets de 3 bits définissant les droit du "_propriétaire_", du "_groupe_", et du "reste du monde".

Tenez, prenez ce tableau ça devrait faire sens :

|                 | Owner |     |     | Group |     |     | Other |     |     |
| --------------- | :---: | :-: | :-: | :---: | :-: | :-: | :---: | :-: | :-: |
| Bits à 1 ou 0   |   r   |  w  |  x  |   r   |  w  |  x  |   r   |  w  |  x  |
| Valeur décimale |   4   |  2  |  1  |   4   |  2  |  1  |   4   |  2  |  1  |
Donc, si l'on voit `rwx` on voit juste ... 4+2+1, donc 7. 

Voici quelques exemples :

|                 | Owner |     |     | Group |     |     | Other |     |     |
| --------------- | :---: | :-: | :-: | :---: | :-: | :-: | :---: | :-: | :-: |
|                 |   r   |  w  |  x  |   r   |  w  |  x  |   r   |  w  |  x  |
| Bits            |   1   |  1  |  1  |   1   |  0  |  1  |   0   |  0  |  0  |
| Valeur décimale |   4   |  2  |  1  |   4   |  2  |  1  |   4   |  2  |  1  |
| Nombre final    |   7   |     |     |   5   |     |     |   0   |     |     |
Donc `750` = "Propriétaire" à tout les droits, "Groupe" peut lire et écrire, et "Reste du monde" ? Rien.
