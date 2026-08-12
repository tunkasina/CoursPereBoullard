# Installer les prérequis
L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce soir c'est un serveur web (Nginx), MariaDB, PHP... Mais quelles versions ? Quels modules ?

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.
### Trouver les prérequis
 - Compatibilité avec Debian ?
 - Les logiciels et leurs versions
 - Les modules PHP nécessaires
Un bonne recherche sur Internet devrait vous aider !

[spoiler]
Cherchez sur le web `insite:mantisbt.org mantis server requisite`. 
Et sur la page des prérequis logiciel, cherchez la `Versions compatibility table`
Cela nous permet de savoir que l'on doit installer :
 + *nginx* (>=1.10.x)comme serveur web
 + *mariadb* (>=5.5.35) comme SGBD
 + *php* (>=7.4) comme interpréteur de script
 + les modules php *mbstring* et *mysql*
 + *php-fpm* (car Nginx, contrairement à Apache, ne gère pas PHP en interne et a besoin d'un gestionnaire de processus externe).

De plus il est clairement dit dans la documentation que l'OS importe peu tant qu'il peut faire tourner les prérequis listés !

Quoi c'est trop vieux jeu pour vous une recherche web ? 
Bon bah demandez à Gemini ou ChatGPT ou Mistra : 
"_Je veux installer Mantis sur NGinx, donne moi les versions exacte des logiciel et modules PHP nécessaire **en me donnant la source, et les liens vers la documentation officielle**_"

En formulant ainsi, vous le forcez à vous donner les informations qu'il a utilisé pour vous régurgiter l'information, et pouvez vérifier que c'est à jour... ou pas !
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
