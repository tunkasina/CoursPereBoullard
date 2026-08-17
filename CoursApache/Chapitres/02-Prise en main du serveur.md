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
<div class="spoiler-content">
<ul>
<li><code>lsb_release -a</code>+A la connexion, vous avez les infos de la version du noyau affichées.</li>
<li><code>apt update && apt upgrade</code> …mise à jour</li>
<li>Si vous avez une erreur du genre <code>[...] n'est pas encore valable (invalide pendant encore 2h 58min 17s)</code>, vous avez un soucis de date ou d'heure. Mettez à l'heure manuellement <code>date -s "2025-10-12 15:07:00"</code>.</li>
<li><code>cat /etc/passwd</code> les comptes qui finissent par <code>/bin/bash</code> ont le droit d'ouvrir une session.</li>
<li>Se connecter en <em>root</em> puis faites <code>passwd</code> pour changer de mot de passe</li>
<li>Faites <code>su - webadmin</code>pour changer d'utilisateur et changer le mot de passe de <em>webadmin</em>.</li>
<li>Eventuellement, faites <code>su -</code> pour passer <em>root</em> depuis <em>webadmin</em>.</li>
</ul>
<p>Vous ne comprenez pas une de ces commandes ou un de ces paramètres ? Cherchez ! Vous devez prendre <strong><em>mal</em></strong> le fait de ne pas savoir, et vouloir corriger cela par vous même. </p>
</div>
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
<div class="spoiler-content">
<ul>
<li><code>ip a</code> si vraiment ...</li>
<li><code>apt install openssh-server</code> (et pas forcément le bundle <code>ssh</code>)</li>
<li><code>systemctl status sshd.service</code> pour vérifier que le service fonctionne</li>
<li><code>ssh root@[VOTRE_IP]</code>, depuis votre <em>desktop</em> pour accéder à votre VM - et constater que <em>root</em> n'a pas le droit de se connecter en ssh par défaut.</li>
<li><code>ssh webadmin@[VOTRE_IP]</code>, pour finalement se connecter à votre VM</li>
<li><code>su -</code> pour <em>élever vos privilèges</em> et passer root.</li>
</ul>
</div>
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
<div class="spoiler-content">
<p>Côté serveur, basculez sur un prompt en tant que <em>webadmin</em>, et :</p>
<ul>
<li><code>ssh-keygen -t ed25519 -C "[UN_COMMENTAIRE_PERTINENT]"</code> + donner un nom explicite</li>
<li>vous devez mettre la clé publique dans <code>.ssh/authorized_keys</code>, si le répertoire n'existe pas, créé le avec les droits <strong>700</strong>.</li>
<li>Puis <code>cat [NOM_EXPLICITE].pub >> .ssh/authorized_keys</code> - et si le fichier n'existais pas, donnez lui les droits <strong>600</strong>.</li>
</ul>
<p>Côté client, pour éviter les soucis d'encodage, on copie le fichier depuis notre poste en le prenant sur le serveur :</p>
<ul>
<li>Se mettre dans son <strong>home</strong>. <code>cd ~</code></li>
<li>Copier : <code>scp webadmin@172.22.69.238:/home/webadmin/[NOM_EXPLICITE] ./.ssh/</code></li>
</ul>
<p>Ensuite on configure le fichier de conf du démon :</p>
<ul>
<li><code>/etc/ssh/shhd_config</code> (il faut dé-commenter <code>PublickeyAuthentication yes</code> et <code>Authorized File .ssh/authorized_keys</code>)</li>
<li><code>systemctl reload sshd.service</code></li>
</ul>
<p>Testez :</p>
<ul>
<li><code>ssh webadmin@[VOTRE_IP] -i .ssh/[NOM_EXPLICITE]</code></li>
</ul>
<p>Modifiez une dernière fois le fichier de conf:</p>
<ul>
<li>Interdisez la connexion par mot de passe: <code>PasswordAuthentication no</code> là où le paramètre apparaît commenté.</li>
<li> <code>systemctl reload sshd.service</code></li>
</ul>
<p>Testez une dernière fois. Respirez.</p>
</div>
</details>

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre à nouveau cette page et **mieux documenter** les soucis que vous avez rencontrés. Obstinez-vous, vous _devez_ être capable de faire tout ça !




