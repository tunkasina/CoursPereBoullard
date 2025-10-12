# Appendice 2: droits Apache
#### Rappel Droits Unix
Je sais que vous vous en souvenez, mais je ne sais pas si on vous l'a expliqué correctement. Les droits Unix (vous savez, **640**, **777**, etc.) ce ne sont que des bits à **0** ou **1** qui autorisent des choses sur un fichier ou un dossier.

Chaque nombre représente un ou plusieurs droits, vous le savez. Mais _pourquoi_ ? Qu'est-ce qui fait que `rwx`=`7` ?

C'est bien plus simple qu'il n'y paraît, si vous savez compter de tête en **binaire** de 1 à 7, vous pouvez faire la conversion de tête sans effort.

Les flags **r**, **w**, **x** sont des bits, qui sont soit à **0**, soit à **1**. Ainsi, si vous voyez affiché `rwx` par exemple, c'est comme si vous lisiez le nombre binaire `111`.  Et si vous voyez `rw-` et bien c'est `110`. Et `r--`, c'est `100`, en binaire.

Et enfin, il y a trois fois 3 bits, chaque paquet de 3 bits définissant les droits du "_propriétaire_", du "_groupe_", et du "_reste du monde_". Donc lorsque l'on fait un `ls -al` :
 - `rwxr-xr-x` c'est en fait `111`, `101`, et `101`, donc _7_, _5_, et _5_
 - `rw-r--r--` c'est en fait `110`, `100`, et `100`, donc _6_, _4_, et _4_.

Si vous avez oublié comment convertir un nombre binaire à trois chiffres, bon, c'est pas très dur, mais c'est très compliqué à écrire. Donc on le fera au tableau, c'est rapide et ça fait de mal à personne.

( Si j'étais un tordu, je pourrais vous dire que chaque bit de la droite vers la gauche double la valeur du précédent en partant de 1, ou encore que c'est `2^n` où `n` est le numéro de la colonne en partant de la droite ... _quoi, on rigole ça va._)

<div class="astuce">Il est obligatoire d'avoir les droits d’exécution pour parcourir un répertoire.</div>

#### Et Apache ?
Sous Debian, _apache_ s’exécute avec l'utilisateur **www-data**, appartenant au groupe **www-data**.

La (fausse) conclusion que tout le monde fait, c'est qu'il faut **www-data** comme ayant les droits sur le répertoire de l'application web que l'on met en ligne. Cela mène à divers possibilités de corruption du serveur, comme :
 - **RCE via Upload** : Shell PHP uploadé puis exécuté avec les droits www-data
 - **LFI to RCE** : Inclusion de fichiers menant à l'exécution de code
 - **Défacement** : Modification des fichiers du site avec droits d'écriture

(Je voulais la joie de googler tout ça et de découvrir les multiples façons que l'on a de se faire bananer en ligne - _sinon, faites du [Root-me](https://www.root-me.org/))

Bref, ce qu'il faut savoir, c'est que généralement on veut les droits minimaux. Il y a deux écoles : 
 - traiter **apache** comme un "other", ignorer **www-data** et configurer les _dossiers_ et _fichiers_ en **775** et **664**.
 - utiliser le groupe **www-data**,  fermer au reste du monde, et configurer les _dossiers_ et _fichiers_ en **750** et **640**.

