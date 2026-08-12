---
title: "Déployer les sources"
parent: "TP Apache"
nav_order: 5
---
# Déployer les sources
Enfin, vous arrivez au bout de votre quête. Vos yeux saignent, vos doigts tremblent, et c'est avec émotions que vous aller mettre en place les sources du logiciel pour enfin le voir fonctionner.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Let's fire this up ! 🚀🚀🚀
### Télécharger les sources
 - Trouver et téléchargez les sources sur votre serveur.

[spoiler]
Sincèrement, je suis désolé que vous ayez eu autant à souffrir. Le proxy de l'IUT c'est pas mon choix, mais c'est ainsi il faut faire avec, IRL ce sera plus facile. Aller configurez moi ce truc :
 - `export http_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`
 - `export https_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`

Et maintenant, téléchargez l'archive (je vous conseille de le faire dans votre `~` en tant que **root**):
 - `wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz`

[/spoiler]

### Déployer les sources
 - Décompressez les et **rangez votre chambre**: on _veux_ un répertoire `/var/www/mantis/` qui contient les sources et pas un sous répertoire supplémentaire !
 - Mettre les bon droits ?! S'appuyer sur l'appendice : [droits et Apache](CoursApache/Appendices/App.02%20droits%20et%20Apache.md).

[spoiler]
La partie facile :
 - `tar -xvf mantisbt-2.27.1.tar.gz`
 - `mv mantisbt-2.27.1 /var/www/mantis`

La partie plus touchy (en tant que **root** !). Pour faire simple, on va donner les droits **750** sur les répertoires, et **640** sur les fichiers. On va appliquer `root:www-data` en propriétaire. Allez dans `/var/www` :
 - `chown -R root:www-data mantis`
 - `chmod -R 750 mantis`
 - `find . -type f -print0 | xargs -0 chmod 640`

Comme on ne connait pas encore l'application, peut être que plus tard on aura à mettre des droits d'écriture sur certain répertoires. On passera alors _temporairement_ ces répertoires à **770**, et nous nous féliciterons d'avoir conservé `www-data` en utilisateur.

[/spoiler]

### Lancer l'installation

<div class="astuce">Faites un snapshot ! Oui maintenant, avant de cliquer quoi que ce soit ! (<i>bon sang mon grand cœur me perdra</i>)</div>

 
 - Lancer l'installation de **Mantis** en complétant les paramètres avec vos notes.


[spoiler]
Remplissez les paramètres d'installation... il vous demande :
 - _Username (for Database)_ : inventez un nouvel utilisateur qui servira à se connecter à la BDD (et **pas** root !)
 - _Password (for Database)_ : donner lui un mot de passe
 - _Admin Username (to create Database if required)_ : **root**, que vous avez configuré en installant _mariadb_
 - _Admin Password (to create Database if required)_ : ...et le mot de passe que vous avez noté

Et lorsque l'on clique sur continuer, il y a quelques soucis ! Il nous indique plusieurs erreurs. **Pas de panique**. Restez bien sur la page.
 - _cannot write /var/www/mantis/config/config_inc.php_ : Aïe ! Donnons lui des droits large sur le répertoire en question, nous corrigerons par la suite : `chmod -R 770 mantis/config`
 - _Database user doesn't have access to the database ( Access denied for user 'mantis_user'@'localhost' )_ : Bon sang, il n'a pas été capable de se donner les bons droits sur sa propre BDD ! Connectons nous sur la BDD : `mariadb -u root -p` et changeons les droits sur la base de donnée visible : `GRANT ALL PRIVILEGES ON bugtracker.* TO 'mantis_user'@'localhost' IDENTIFIED BY '[VOTRE_MOT_DE_PASSE_POUR_CET_USER]';` (attention à la fatigue, mettez bien le **bon** user, le **bon** mot de passe !)

Maintenant, faites "_précédent_" sur votre navigateur, puis "_Install/Upgrade Database_".... Et il devrait vous indiquer en bas "_MantisBT was installed successfully. Continue to log in._"

Enfin, cliquez sur _Continuez_ et authentifiez vous, comme dans la doc, avec comme login `administrator` et mot de passe `root`.

**Et si ça ne marche pas pour moi?**.
 - Reprenez le snapshot précédent où tout allait bien
 - Aidez vous des logs d'apache : `tail -f /var/log/apache2/error.log`

[/spoiler]

## Final
Bon sang ! Déjà fini ? Mais comment diantre ?! Super, vous avez le temps de **restaurer votre snapshot et recommencer** !

Après ça, il ne nous reste plus que la partie "consolidation", où l'on va tenter de renforcer notre machine vis-à-vis du monde hostile qui nous entoure.

[06-Consolidation](./CoursApache/Chapitres/06-Consolidation.md)

