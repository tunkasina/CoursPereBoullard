L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce sera forcément Apache, MariaDB, PHP…  Mais quelles versions ? Quelles modules ? Quels sont les modules obligatoires, ou non ?
## Consignes
### Trouver les prérequis
 - Compatibilité avec Debian
 - Les logiciels et leurs versions
 - les modules et leurs versions

> [!soluce]- Solution
> `insite:mantisbt.org mantis server requisite` qui nous permet de savoir que l'on doit installer :
> _apache2_ (>=2.4.13) comme serveur web
> _mariadb_ (>=5.5.35) comme SGBD
> _php_ (>=7.4) comme interpréteur de script
> + les modules php _mbstring_ et _mysql_ 

### Installer les prérequis
 - …

> [!soluce]- Solution
> Pour installer apache : `apt install apache2`
> Pour installer mariadb : `apt install mariadb`
> Pour installer php et ses modules : `apt install php php-mbstring php-mysql`

### Configurer/Vérifier les prérequis
 - Vérifier que _apache_ fonctionne
 - Vérifier que _mariadb_ fonctionne
 - Vérifier que _php_ fonctionne
