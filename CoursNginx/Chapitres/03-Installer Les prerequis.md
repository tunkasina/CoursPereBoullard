# Installer les prérequis
L'application Web que nous déployons a besoin de prérequis sur le serveur. Pour **Mantis**, il faut lire sa doc et trouver ses besoins.  

(_Spoiler Alert?_) Ce sera Nginx, MariaDB, PHP… Mais quelles versions ? Quels modules ?

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Trouver les prérequis
 - Compatibilité avec Debian ?
 - Les logiciels et leurs versions (Nginx au lieu d'Apache)
 - Les modules PHP nécessaires

[spoiler]
Pour MantisBT :
 + **Nginx** comme serveur web
 + **MariaDB** comme SGBD
 + **PHP** (>= 7.4)
 + Modules PHP : `php-fpm` (indispensable pour Nginx), `php-mbstring`, `php-mysql`, `php-xml`, `php-curl`
[/spoiler]

### Installer les prérequis
 - Installez les paquets nécessaires.

[spoiler]
 - Pour installer Nginx : `apt install nginx`
 - Pour installer MariaDB : `apt install mariadb-server`
 - Pour installer PHP-FPM (le moteur PHP pour Nginx) et les modules : 
   `apt install php-fpm php-mbstring php-mysql php-xml php-curl`
[/spoiler]

### Vérifier les prérequis
 - Vérifier que _nginx_ fonctionne
 - Vérifier que _mariadb_ fonctionne
 - Vérifier que _PHP-FPM_ fonctionne

[spoiler]
 - `systemctl status nginx`
 - `systemctl status mariadb`
 - `systemctl status php8.2-fpm` (vérifiez le numéro de version installé)
 - Pour tester Nginx : allez sur `http://[ip_serveur]`, vous devriez voir la page "Welcome to nginx!".
[/spoiler]

## Final
**STOP** ! On configurera tout ça au prochain chapitre. Comme d'habitude : si vos notes ne suffisent pas, **restaurez et recommencez** !

[04-Configurer les prerequis](./04-Configurer%20les%20prerequis.md)
