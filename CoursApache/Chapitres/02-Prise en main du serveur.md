Dans le cadre de votre mise en production, vous recevrez une machine virtuelle sur une infrastructure distante. Lorsque vous louez un serveur "sans option" auprès d'un hébergeur, c'est exactement ce que vous aurez; une machine (Linux le plus souvent) fraîchement installée, mais vide de logiciels. Vous avez dû recevoir un mail avec le nom, et les identifiants nécessaire pour accéder à cette machine. Je n'ai pas encore l'URL au moment où j'écris ces lignes, vous les aurez en cours directement.

<div class="tip">Faites un snapshot !</div>

Enfin, prenez des _notes_ de telle façon que _vous soyez capable de tout refaire sans mon support_!
## Consignes
### Prise en main
 - Trouvez la version et le niveau de mise à jour de votre serveur
 - Mettez à jour votre serveur
 - Trouvez tout les utilisateurs autorisés à ouvrir une session sur le serveur
 - Modifiez les mots de passe de ces utilisateurs

<details class="soluce">
<summary>Solution</summary>
`lsb_release -a` + info à la connexion
`apt update && apt upgrade`
`cat /etc/passwd` + users avec un `/bin/bash` à la fin
`passwd` sur root, et test avec `su - webadmin` puis `su -` et validation du mot de passe défini
</details>

### Installer SSH
 - Trouvez l'IP de votre serveur
 - Installer SSH
 - Vérifiez que le démon fonctionne
 - Tentez une connexion avec chaque utilisateurs de votre système
 - Trouvez comment élever vos privilèges et être root sur le système via SSH

<details class="soluce">
<summary>Solution</summary>
`ip a` si vraiment...
`apt install openssh-server`
`systemctl status sshd.service`
`ssh root@172.22.69.238`
`ssh webadmin@172.22.69.238`
`su -`
</details>

### Configurer la connexion par clé
 - Générez un jeu de clé SSH
 - Mettez en œuvre votre clé publique sur le serveur et configurez SSH pour
 - Configurer PuTTy ou n'importe quel autre client SSH pour cette connexion
 - Validez votre capacité à prendre la main

<details class="soluce">
<summary>Solution</summary>
Côté serveur : Basculer sur un prompt en tant que _webadmin_  
`ssh-keygen -t ed25519 -C "pereBoullard"` + donner un nom explicite  
`cat nomExplicite.pub >> .ssh\authorized_keys`  
Côté client : Pour éviter les soucis d'encodage, on copie le fichier  
`scp webadmin@172.22.69.238:/home/webadmin/pereBoullard ./.ssh/`  
Ensuite on configure le fichier `/etc/ssh/shhd_config`  
Et on recharge le fichier de conf du démon `systemctl reload sshd.service`
</details>

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre à nouveau cette page. Obstinez-vous, vous _devez_ être capable de faire tout ça !
