L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce sera forcément Apache, MariaDB, PHP...  Mais quelles versions ? Quelles modules ? Quels sont les modules obligatoires, ou non ?
## Consignes
### Trouver les prérequis
 - Compatibilité avec Debian
 - Les logiciels et leurs versions
 - les modules et leurs versions

> [!soluce]- Solution
> `insite:mantisbt.org mantis server requisite`

### Installer les prérequis
 - Apache
 - MariaDB
 - 