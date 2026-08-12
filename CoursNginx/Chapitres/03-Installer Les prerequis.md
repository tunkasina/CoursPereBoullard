# Installer les prérequis
L'application Web que nous déployons (**Mantis**) a besoin de prérequis sur le serveur (un SGBD, un interpréteur de script et notre serveur web Nginx).

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires...

### Trouver et installer les prérequis
 - Quels sont les prérequis nécessaires pour faire tourner Mantis (identiques à la voie Apache : SGBD, PHP et ses modules) ?
 - Installez **MariaDB** et **PHP** (avec les modules nécessaires).

[spoiler]
Pour Mantis :
 - `apt install mariadb-server`
 - `apt install php php-mysql php-mbstring php-fpm` (Remarquez l'ajout de `php-fpm` nécessaire pour que Nginx puisse dialoguer avec PHP, contrairement à Apache qui gère souvent cela en module direct).
[/spoiler]

### Installer Nginx
 - Installez **Nginx**.
 - Vérifiez que le démon fonctionne et écoute sur le port 80.

[spoiler]
 - `apt install nginx`
 - `systemctl status nginx.service`
 - `ss -tlnp | grep 80`
[/spoiler]

### Vérifier les prérequis
 - Vérifiez que MariaDB et PHP-FPM fonctionnent.
 - Créez une page `index.php` de test sous `/var/www/html/index.php` contenant `<?php phpinfo(); ?>` et vérifiez que Nginx / PHP-FPM interprète bien le code.

[spoiler]
 - `systemctl status mariadb.service`
 - `systemctl status php8.2-fpm.service` (ou version correspondante)
 - Pour lier Nginx à PHP-FPM, il faudra configurer le bloc `location ~ \.php$` dans votre vhost en passant par un socket (`unix:/run/php/php-fpm.sock`).
[/spoiler]

## Final
Vos prérequis (MariaDB, PHP-FPM, Nginx) sont installés. Dans la suite, nous allons configurer les services et déployer Mantis.

[04-Configurer les prerequis](./04-Configurer%20les%20prerequis.md)
