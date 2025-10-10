Dans le cadre de votre mise en production, vous recevrez une machine virtuelle sur une infrastructure distante. Lorsque vous louez un serveur "sans option" auprès d'un hébergeur, c'est exactement ce que vous aurez; une machine (Linux le plus souvent) fraîchement installée, mais vide de logiciels. Vous avez dû recevoir un mail avec le nom, et les identifiants nécessaire pour accéder à cette machine. Je n'ai pas encore l'URL au moment où j'écris ces lignes, vous les aurez en cours directement.

<div class="astuce">Faites un snapshot !</div>

Enfin, prenez des _notes_ de telle façon que _vous soyez capable de tout refaire sans mon support_!
## Consignes
### Prise en main
 - Trouvez la version et le niveau de mise à jour de votre serveur
 - Mettez à jour votre serveur
 - Trouvez tout les utilisateurs autorisés à ouvrir une session sur le serveur
 - Modifiez les mots de passe de ces utilisateurs

[spoiler]
`lsb_release -a`+A la connexion, vous avez les infos de la version du noyau affichées.
`apt update && apt upgrade` …mise à jour
`cat /etc/passwd` les comptes qui finissent par `/bin/bash` ont le droit d'ouvrir une session.
Se connecter en _root_ ûis faites `passwd` pour changer de mot de passe
Faites `su - webadmin`pour changer d'utilisateur et changer le mot de passe de _webadmin_.
Eventuellement, faites `su -` pour passer _root_ depuis _webadmin_.
[/spoiler]

### Installer SSH
 - Trouvez l'IP de votre serveur
 - Installer SSH
 - Vérifiez que le démon fonctionne
 - Tentez une connexion avec chaque utilisateurs de votre système
 - Trouvez comment élever vos privilèges et être root sur le système via SSH

<details class="soluce"><summary>Solution</summary>
<code>ip a</code> si vraiment...<br/>
<code>apt install openssh-server</code><br/>
<code>systemctl status sshd.service</code><br/>
<code>ssh root@172.22.69.238</code><br/>
<code>ssh webadmin@172.22.69.238</code><br/>
<code>su -</code><br/>
</details>

[spoiler]

Ceci est un spoiler caché !

`Ceci est un spoiler caché !`

_Ceci est un spoiler caché !_

Ceci est un **spoiler** caché !

[/spoiler]

### Configurer la connexion par clé
 - Générez un jeu de clé SSH
 - Mettez en œuvre votre clé publique sur le serveur et configurez SSH pour
 - Configurer PuTTy ou n'importe quel autre client SSH pour cette connexion
 - Validez votre capacité à prendre la main

<details class="soluce"><summary>Solution</summary>
Côté serveur : Basculer sur un prompt en tant que _webadmin_  <br/>
<code>ssh-keygen -t ed25519 -C "pereBoullard"</code> + donner un nom explicite  <br/>
<code>cat nomExplicite.pub >> .ssh\authorized_keys</code>  <br/>
Côté client : Pour éviter les soucis d'encodage, on copie le fichier  <br/>
<code>scp webadmin@172.22.69.238:/home/webadmin/pereBoullard ./.ssh/</code><br/>
Ensuite on configure le fichier <code>/etc/ssh/shhd_config</code><br/>
Et on recharge le fichier de conf du démon <code>systemctl reload sshd.service</code><br/>
</details>

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre à nouveau cette page. Obstinez-vous, vous _devez_ être capable de faire tout ça !
