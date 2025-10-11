### Appendice 1: mysql_secure_installation
Voici toutes les interrogations du script et leur signification:

 - `Enter current password for root (enter for none):` : il demande le mot de passe root actuel de _mariadb_.
 - `Switch to unix_socket authentication [Y/n]` : il demande si l'on veux utiliser des utilisateur du système comme utilisateurs dans _mariadb_. En général, on répond non, et on aura un utilisateur défini dans _mariadb_ directement.
 - `Change the root password? [Y/n]` : Attention, le script ment! (enfin, disons, n'a pas le même point de vue que moi...) Votre compte root n'est pas protégé du tout. Définissez ici un mot de passe **root** pour le serveur _mariadb_.
 - `Remove anonymous users? [Y/n]`  : je sais même pas pourquoi il demande. Oui bien sûr !
 - `Disallow root login remotely? [Y/n]` : sincèrement, oui. Cela force une personne qui veut être connecté en tant que **root** sur _mariadb_ à être en local sur le système.
 - `Remove test database and access to it? [Y/n]` : pas besoin de garder 
`
`