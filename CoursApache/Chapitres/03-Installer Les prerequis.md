L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce sera forcément Apache, MariaDB, PHP…  Mais quelles versions ? Quelles modules ? Quels sont les modules obligatoires, ou non ?
## Consignes
### Trouver les prérequis
 - Compatibilité avec Debian
 - Les logiciels et leurs versions
 - les modules et leurs versions

> [!soluce]- Solution
> `insite:mantisbt.org mantis server requisite` qui nous permet de savoir que l'on doit installer :
> +  _apache2_ (>=2.4.13) comme serveur web
> + _mariadb_ (>=5.5.35) comme SGBD
> + _php_ (>=7.4) comme interpréteur de script
> + les modules php _mbstring_ et _mysql_ 

### Installer les prérequis
 - Installer les prérequis que vous avez précédemment trouvés

> [!soluce]- Solution
> Pour installer apache : `apt install apache2`
> Pour installer mariadb : `apt install mariadb-server`
> Pour installer PHP et ses modules : `apt install php php-mbstring php-mysql`
> (_oui on est d'accord, pas de quoi se taper la tête contre les murs_)

### Configurer/Vérifier les prérequis
 - Vérifier que _apache_ fonctionne
 - Vérifier que _mariadb_ fonctionne
 - Vérifier que _PHP_ fonctionne

> [!soluce]- Solution
> Dans un premier temps, on vérifie que les différents démons sont fonctionnels : `systemctl status apache2.service`, `systemctl status mariadb.service` - qui affiche le statut et un extrait des logs.
> Ensuite, une petite visite sur `http://[ip de votre serveur]` vous permet de voir que _apache_ vous rend bien une page web
> Pour vérifier que _PHP_ fonctionne, il faut simplement se créer une page **/var/www/html/index.php** avec comme contenu `<?php  phpinfo();`. Y accéder via `http://[ip de votre serveur]/index.php` devrait vous renvoyer une bien belle page d'info sur votre interpréteur PHP et ses modules.
> Enfin pour _mariadb_ on va devoir exécuter le script **mysql_secure_installation**, en comprendre le contenu, et répondre aux questions (plus de détails via [ce lien](mysql_secure_installation.md)).


> Pour installer PHP et ses modules : `apt install php php-mbstring php-mysql`
> (_oui on est d'accord, pas de quoi se taper la tête contre les murs_)
