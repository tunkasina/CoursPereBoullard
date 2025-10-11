### Déployer les sources
Enfin, vous arrivez au bout de votre quête. Vos yeux saignent, vos doigts tremblent, et c'est avec émotions que vous aller mettre en place les sources du logiciel pour enfin le voir fonctionner.

<div class="astuce">Faites un snapshot !</div>

## Consignes
🚀🚀🚀

### Télécharger les sources
 - Trouver et téléchargez les sources sur votre serveur.

[spoiler]
Sincèrement, je suis désolé que vous ayez eu autant à souffrir. Le proxy de l'IUT c'est pas mon choix, mais c'est ainsi il faut faire avec, IRL ce sera plus facile. Aller configurer moi ce truc :
 - `export http_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`
 - `export https_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`

Et maintenant, téléchargez l'archive :
 - `wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz`

 - Le script **mysql_secure_installation** mérite sa propre page, pour en comprendre le contenu et cela se trouve via [ce lien](https://tunkasina.github.io/CoursPereBoullard/#/./CoursApache/Chapitres/App.01%20mysql_secure_installation.md).
 - On ne créé pas de base de donnée ou de compte particulier, on va utiliser notre compte **root** de _mariadb_ au moment critique.
[/spoiler]

### Déployer les sources
 - Décompressez les et **rangez votre chambre**: on _veux_ un répertoire `/var/www/mantis/` qui contient les sources et pas un sous répertoire supplémentaire !

[spoiler]
 - Le script **mysql_secure_installation** mérite sa propre page, pour en comprendre le contenu et cela se trouve via [ce lien](https://tunkasina.github.io/CoursPereBoullard/#/./CoursApache/Chapitres/App.01%20mysql_secure_installation.md).
 - On ne créé pas de base de donnée ou de compte particulier, on va utiliser notre compte **root** de _mariadb_ au moment critique.
[/spoiler]


