# Prise en main du serveur
Dans le cadre de votre mise en production, vous recevrez une machine virtuelle sur une infrastructure distante. Lorsque vous louez un serveur "sans option" auprès d'un hébergeur, c'est exactement ce que vous aurez; une machine fraîchement installée, mais vide de logiciels.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez **par vous même et par tout les moyens nécessaires**, les commandes à taper pour faire chacune de ces actions ! 

### Prise en main
 - Trouvez la version et le niveau de mise à jour de votre serveur
 - Mettez à jour votre serveur
 - Trouvez tout les utilisateurs autorisés à ouvrir une session sur le serveur
 - Modifiez les mots de passe de ces utilisateurs

[spoiler]
 - `lsb_release -a` ou `cat /etc/debian_version`
 - `apt update && apt upgrade`
 - `cat /etc/passwd` (cherchez `/bin/bash` ou `/bin/zsh`)
 - `passwd [utilisateur]`
[/spoiler]

### Installer SSH
 - Trouvez l'IP de votre serveur
 - Installer SSH
 - Vérifiez que le démon fonctionne
 - Utilisez un client **SSH** pour tenter une connexion
 - Trouvez comment élever vos privilèges (passer root)

[spoiler]
 - `ip a`
 - `apt install openssh-server`
 - `systemctl status ssh`
 - `ssh webadmin@[VOTRE_IP]`
 - `su -` ou `sudo -i`
[/spoiler]

### Configurer la connexion par clé
 - Générez un jeu de clé SSH
 - Mettez en œuvre votre clé publique sur le serveur et votre clé privée sur votre client **SSH**.
 - Validez votre capacité à prendre la main sans mot de passe.
 - Désactivez l'authentification par mot de passe pour plus de sécurité.

[spoiler]
Côté serveur :
 - `mkdir -p ~/.ssh && chmod 700 ~/.ssh`
 - `nano ~/.ssh/authorized_keys` (y coller la clé publique)
 - `chmod 600 ~/.ssh/authorized_keys`

Côté configuration SSH (`/etc/ssh/sshd_config`) :
 - `PubkeyAuthentication yes`
 - `PasswordAuthentication no`
 - `systemctl reload ssh`
[/spoiler]

## Final
Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide. Vous _devez_ être capable de faire tout ça les yeux fermés !

[03-Installer Les prerequis](./03-Installer%20Les%20prerequis.md)
