# Installer les prérequis
L'application Web que nous déployons (**Mantis**) a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez. Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce soir c'est un serveur web (Nginx), MariaDB, PHP... Mais quelles versions ? Quels modules ?

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.

### Trouver les prérequis
 - Compatibilité avec Debian ?
 - Les logiciels et leurs versions (Nginx, MariaDB, PHP)
 - Les modules PHP nécessaires (notamment pour la base de données et les chaînes de caractères, sans oublier PHP-FPM pour Nginx)

[spoiler]
Cherchez sur le web `insite:mantisbt.org mantis server requisite`. Cela nous permet de savoir que l'on doit installer :
 + *nginx* comme serveur web
 + *mariadb* (>=5.5.35) comme SGBD
 + *php* (>=7.4) comme interpréteur de script
 + les modules php *mbstring* et *mysql*
 + *php-fpm* (car Nginx, contrairement à Apache, ne gère pas PHP en interne et a besoin d'un gestionnaire de processus externe).
[/spoiler]

### Installer les prérequis
 - Installez les prérequis que vous avez précédemment trouvés.

[spoiler]
 - Pour installer Nginx : `apt install nginx`
 - Pour installer MariaDB : `apt install mariadb-server`
 - Pour installer PHP et ses modules : `apt install php php-mbstring php-mysql php-fpm`
[/spoiler]

### Vérifier les prérequis
 - Vérifiez que Nginx fonctionne.
 - Vérifiez que MariaDB fonctionne.
 - Vérifiez que PHP et PHP-FPM fonctionnent.

[spoiler]
 - Dans un premier temps, on vérifie que les démons sont fonctionnels : `systemctl status nginx.service`, `systemctl status mariadb.service`, `systemctl status php-fpm` (ou version correspondante).
 - Ensuite, pour tester l'interprétation PHP avec Nginx, créez une page `/var/www/html/index.php` avec `<?php phpinfo(); ?>` (attention, sous Nginx il vous faudra configurer le bloc `location ~ \.php$` pointant vers le socket PHP-FPM pour l'afficher correctement).
[/spoiler]

## Final
Et maintenant ... ? **STOP** ! On configurera à [04-Configurer les prerequis](./04-Configurer%20les%20prerequis.md) !

Comme la dernière fois, si vos notes ne vous permettent pas de refaire cette partie... **restaurez votre snapshot et recommencez** !
