---
title: "Prise en main du serveur"
parent: "TP Apache"
nav_order: 2
---
# Prise en main du serveur
Dans le cadre de votre mise en production, vous recevrez une machine virtuelle sur une infrastructure distante. Lorsque vous louez un serveur "sans option" auprès d'un hébergeur, c'est exactement ce que vous aurez; une machine (Linux le plus souvent) fraîchement installée, mais vide de logiciels. Vous avez dû recevoir un mail avec le nom, et les identifiants nécessaire pour accéder à cette machine.

<div class="astuce">Faites un snapshot !</div>

Enfin, prenez des _notes_ de telle façon que _vous soyez capable de tout refaire sans mon support_!
## Consignes
Evidemment, vous chercherez **par vous même et par tout les moyens nécessaires**, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.
### Prise en main
Vous ne savez pas comment faire? C'est normal, mais le rester sans rien faire, non ! Internet, les amis, votre prof, cherchez, _cherchez_, **cherchez** !
 - Trouvez la version et le niveau de mise à jour de votre serveur
 - Mettez à jour votre serveur
 - Trouvez tout les utilisateurs autorisés à ouvrir une session sur le serveur
 - Modifiez les mots de passe de ces utilisateurs

<details class="spoiler">
<summary>Solution / Indice</summary>
<ul>
<li>`lsb_release -a`+A la connexion, vous avez les infos de la version du noyau affichées.</li>
<li>`apt update && apt upgrade` …mise à jour</li>
<li>Si vous avez une erreur du genre `[...] n'est pas encore valable (invalide pendant encore 2h 58min 17s)`, vous avez un soucis de date ou d'heure. Mettez à l'heure manuellement `date -s "2025-10-12 15:07:00"`.</li>
<li>`cat /etc/passwd` les comptes qui finissent par `/bin/bash` ont le droit d'ouvrir une session.</li>
<li>Se connecter en _root_ puis faites `passwd` pour changer de mot de passe</li>
<li>Faites `su - webadmin`pour changer d'utilisateur et changer le mot de passe de _webadmin_.</li>
<li>Eventuellement, faites `su -` pour passer _root_ depuis _webadmin_.</li>
</ul>
<p>Vous ne comprenez pas une de ces commandes ou un de ces paramètres ? Cherchez ! Vous devez prendre **_mal_** le fait de ne pas savoir, et vouloir corriger cela par vous même. </p>
</details>

### Installer SSH
\[...] cherchez, _cherchez_, **cherchez** !
 - Trouvez l'IP de votre serveur
 - Installer SSH
 - Vérifiez que le démon fonctionne
 - Utilisez un client **SSH** pour tenter une connexion avec chaque utilisateurs de votre système (Comme **PuTTY** ou [Windows Terminal](../Appendices/App.06 Windows Terminal.md))
 - Trouvez comment élever vos privilèges et être root sur le système via SSH

<details class="spoiler">
<summary>Solution / Indice</summary>
<ul>
<li>`ip a` si vraiment ...</li>
<li>`apt install openssh-server` (et pas forcément le bundle `ssh`)</li>
<li>`systemctl status sshd.service` pour vérifier que le service fonctionne</li>
<li>`ssh root@[VOTRE_IP]`, depuis votre _desktop_ pour accéder à votre VM - et constater que _root_ n'a pas le droit de se connecter en ssh par défaut.</li>
<li>`ssh webadmin@[VOTRE_IP]`, pour finalement se connecter à votre VM</li>
<li>`su -` pour _élever vos privilèges_ et passer root.</li>
</ul>
</details>

### Configurer la connexion par clé
Attention ici c'est le moment ou typiquement, on s'enferme dehors. Heureusement vous, vous avez la main directement dans l'interface web de Proxmox, mais dans la vraie vie, c'est rarement le cas... Donc il faut être **ri-gou-reux**. 

_"- Oui mais m'sieur c'est quoi rigoureux?"_
Bonne question Jean-michel-à-peu-près. Rigoureux dans notre contexte, ça veux dire être sûr d'avoir compris ce que l'on est en train de faire, et donc être certain que l'ordre de nos étapes est juste et pertinent.

 - Générez un jeu de clé SSH
 - Mettez en œuvre votre clé publique sur le serveur et votre clé privée sur votre client **SSH**.
 - Validez votre capacité à prendre la main

<details class="spoiler">
<summary>Solution / Indice</summary>
<p>Côté serveur, basculez sur un prompt en tant que _webadmin_, et :</p>
<ul>
<li>`ssh-keygen -t ed25519 -C "[UN_COMMENTAIRE_PERTINENT]"` + donner un nom explicite</li>
<li>vous devez mettre la clé publique dans `.ssh/authorized_keys`, si le répertoire n'existe pas, créé le avec les droits **700**.</li>
<li>Puis `cat [NOM_EXPLICITE].pub >> .ssh/authorized_keys` - et si le fichier n'existais pas, donnez lui les droits **600**.</li>
</ul>
<p>Côté client, pour éviter les soucis d'encodage, on copie le fichier depuis notre poste en le prenant sur le serveur :</p>
<ul>
<li>Se mettre dans son **home**. `cd ~`</li>
<li>Copier : `scp webadmin@172.22.69.238:/home/webadmin/[NOM_EXPLICITE] ./.ssh/`</li>
</ul>
<p>Ensuite on configure le fichier de conf du démon :</p>
<ul>
<li>`/etc/ssh/shhd_config` (il faut dé-commenter `PublickeyAuthentication yes` et `Authorized File .ssh/authorized_keys`)</li>
<li>`systemctl reload sshd.service`</li>
</ul>
<p>Testez :</p>
<ul>
<li>`ssh webadmin@[VOTRE_IP] -i .ssh/[NOM_EXPLICITE]`</li>
</ul>
<p>Modifiez une dernière fois le fichier de conf:</p>
<ul>
<li>Interdisez la connexion par mot de passe: `PasswordAuthentication no` là où le paramètre apparaît commenté.</li>
<li> `systemctl reload sshd.service`</li>
</ul>
<p>Testez une dernière fois. Respirez.</p>
</details>

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre à nouveau cette page et **mieux documenter** les soucis que vous avez rencontrés. Obstinez-vous, vous _devez_ être capable de faire tout ça !




