L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce sera forcément Apache, MariaDB, PHP...  Mais quelles versions ? Quelles modules ? Quels sont les modules obligatoires, ou non ?
## Consignes
### Trouver les prérequis
 - Compatibilité avec Debian
 - Les logiciels et leurs versions
 - les modules et leurs versions

> [!soluce]- Solution
> `insite:mantisbt.org mantis server requisite` qui nous permet de savoir que l'on peut installer :
> _apache2_ (>=2.4.13) comme serveur web
> _mariadb_ (>=5.5.35) comme SGBD
> _php_ (>=7.4) comme interpréteur de script
> + les modules php _mbstring_ et _mysql_ 

### Installer les prérequis
 - …

> [!soluce]- Solution
> `apt install apache2`
> `apt install mariadb`
