Dans le cadre de votre mise en production, vous recevrez une machine virtuelle sur une infrastructure distante. Lorsque vous louez un serveur "sans option" auprès d'un hébergeur, c'est exactement ce que vous aurez; une machine (Linux le plus souvent) fraîchement installée, mais vide de logiciels. Vous avez dû recevoir un mail avec le nom, et les identifiants nécessaire pour accéder à cette machine.

<div class="astuce">Faites un snapshot !</div>

Enfin, prenez des _notes_ de telle façon que _vous soyez capable de tout refaire sans mon support_!
## Consignes
Evidemment, vous chercherez par vous même et par tout les moyens nécessaires, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.
### Prise en main
 - Trouvez la version et le niveau de mise à jour de votre serveur
 - Mettez à jour votre serveur
 - Trouvez tout les utilisateurs autorisés à ouvrir une session sur le serveur
 - Modifiez les mots de passe de ces utilisateurs

[spoiler]
 - `lsb_release -a`+A la connexion, vous avez les infos de la version du noyau affichées.
 - `apt update && apt upgrade` …mise à jour
 - `cat /etc/passwd` les comptes qui finissent par `/bin/bash` ont le droit d'ouvrir une session.
 - Se connecter en _root_ ûis faites `passwd` pour changer de mot de passe
 - Faites `su - webadmin`pour changer d'utilisateur et changer le mot de passe de _webadmin_.
 - Eventuellement, faites `su -` pour passer _root_ depuis _webadmin_.
[/spoiler]

### Installer SSH
 - Trouvez l'IP de votre serveur
 - Installer SSH
 - Vérifiez que le démon fonctionne
 - Tentez une connexion avec chaque utilisateurs de votre système
 - Trouvez comment élever vos privilèges et être root sur le système via SSH

[spoiler]
 - `ip a` si vraiment ...
 - `apt install openssh-server` (et pas forcément le bundle `ssh`)
 - `systemctl status sshd.service` pour vérifier que le service fonctionne
 - `ssh root@172.22.69.238`, depuis votre _desktop_ pour accéder à votre VM - et constater que _root_ n'a pas le droit de se connecter en ssh par défaut.
 - `ssh webadmin@172.22.69.238`, pour finalement se connecter à votre VM
 - `su -` pour _élever vos privilèges_ et passer root.
[/spoiler]

### Configurer la connexion par clé
 - Générez un jeu de clé SSH
 - Mettez en œuvre votre clé publique sur le serveur et configurez SSH pour
 - Configurer PuTTy ou n'importe quel autre client SSH pour cette connexion
 - Validez votre capacité à prendre la main

[spoiler]
Côté serveur, basculez sur un prompt en tant que _webadmin_, et :
 - `ssh-keygen -t ed25519 -C "pereBoullard"` + donner un nom explicite
 - `cat nomExplicite.pub >> .ssh\authorized_keys`
Côté client, pour éviter les soucis d'encodage, on copie le fichier depuis le serveur sur notre poste :
 - `scp webadmin@172.22.69.238:/home/webadmin/pereBoullard ./.ssh/`
Ensuite on configure le fichier de conf du démon :
 - `/etc/ssh/shhd_config`
 - `systemctl reload sshd.service`
Testez, respirez.
[/spoiler]

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre à nouveau cette page. Obstinez-vous, vous _devez_ être capable de faire tout ça !
