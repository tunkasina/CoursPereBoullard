---
title: "Consolidation"
parent: "TP Apache"
nav_order: 6
---
# Consolidation
On appelle **Consolidation** le fait de renforcer notre serveur face aux attaques potentielles.Bien entendu, vous ne savez pas comment s'organise une attaque et heureusement, j'ai pensé à vous.

Allez sur [une attaque informatique](../Appendices/App.04 attaque informatique.md) pour en savoir plus, avant de faire la suite !

<div class="astuce">Faites un snapshot !</div>

## Consignes
Vous n'aurez pas le temps de tout faire. Vous devrez choisir parmi les rubriques suivantes, ce qui vous intéresse plus ... Vous ne serez pas évalué dessus, bien sûr !

### Passif
Cela consiste à limiter ce que l'on offre de manière passive, aux potentiels agresseurs. Voyez cela comme le fait de ne pas laisser la clé sur la serrure, mais aussi comme le fait d'éviter de mettre une grosse flèche rouge vers votre coffre-fort, ou de laisser la porte du jardin ouverte.

#### Post installation
 - Appliquez les instructions post-installation de **Mantis** 
 - Contrôler les droits utiles et nécessaire sur `/var/www/mantis`

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>A plusieurs endroits **Mantis** nous indique des choses à faire :</p>
<ul>
<li>_**Attention :** vous devriez désactiver le compte « administrator » par défaut ou changer son mot de passe._</li>
<li>_**Attention :** le répertoire « admin » par défaut devrait être supprimé ou son accès devrait être restreint._</li>
</ul>
<p>Puise que c'est le logiciel qui le dit, faites ! Et tant que vous avez le nez dans votre _shell_ pensez à modifier les droits que l'on avait un peu trop ouvert dans `/var/www/mantis/config`, vous vous souvenez ? Allez dans `mantis`</p>
<ul>
<li>`chmod -R 750 config`</li>
<li>`find . -type f -print0 | xargs -0 chmod 640`</li>
</ul>
<p>Et comme on ne supprime rien sans avoir la possibilité de le restaurer :</p>
<ul>
<li>`mv admin ~/admin.old`</li>
</ul>
</div>
</details>

#### Discrétion
 - Obtenez un 404 et trouvez les infos en trop
 - Interrogez votre serveur pour obtenir les **HEADERS** renvoyés
 - Diminuez la signature d'**Apache**

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<ul>
<li>Essayer `http://[VOTRE_IP]/test` : vous obtenez des choses comme _Apache/2.4.65 (Debian) OpenSSL/3.0.17 Server at 172.22.69.238 Port 443_</li>
<li>Faites **F12** pour avoir vos outils de dev', et cherchez dans **Réseau**. Prenez la première requête **GET** et regardez les **en-têtes**. Vous devriez trouvez quelque chose comme _Server: Apache/2.4.65 (Debian) OpenSSL/3.0.17_</li>
</ul>
<p>Ce genre d'info, c'est le exactement ce que vous ne _voulez pas_ montrer. Pour empêcher ce comportement, éditez `/etc/apache2/conf-available/security.conf`, et passez les paramètres `ServerTokens` à `Prod` et `ServerSignature` à `Off`.</p>
<p>Dans le temps, **PHP** affichait ses infos de version à chaque plantage. Soyez méfiant, si vous voyez un numéro de version apparaître lors de vos session de code, c'est qu'il y a une option quelque part à désactiver !</p>
</div>
</details>

#### Seulement l'utile et le nécessaire
 - Vérifier vos port ouverts vers l’extérieur

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Heureusement vous n'avez rien d'ouvert ! Voici comment le vérifier:</p>
<ul>
<li>`ss -tul`</li>
</ul>
<p>Vous verrez toutes les écoutes (`l`) tcp (`t`) et udp (`u`). Et dedans on constate que **SSH** écoute sur toutes les interface IPv4 (`0.0.0.0`), **mariadb** seulement la boucle locale (`127.0.0.1`), et **Apache** écoute toutes les interfaces possibles (`*:http`, `*:https`) (_oui Apache est très à l'écoute_ ).</p>
</div>
</details>

Si vous avez été attentif, vous comprendrez qu'en étant soucieux de rester discret, et de n'exposer que l'utile et le nécessaire, nous agissons sur l'étape de **reconnaissance** et l'**exploitation** d'un adversaire.
### Actif
Cela consiste à mener des actions de protection, afin d'augmenter la résilience de notre système. C'est la maintenance de notre système, et des bonnes pratiques, de bonne santé.
 - Mettre en place un script de sauvegarde de la base de donnée
 - Mettre en place un script de vérification de mise à jours dans le `.bashrc` (qui s'affichera donc dès que vous vous connectez sur le serveur)

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Un simple script de sauvegarde sera déjà un bon début ! Les admins prévoyants en mettent souvent quelques-un à divers étages ... </p>
<p>Directement dans le home de root, créez un script **shell** :</p>
<ul>
<li>`nano backup_mariadb.sh`</li>
</ul>
<p>Et mettre dedans (avec explications ligne à ligne !):</p>
<p>`#!/bin/bash`</p>
<p>_signifie que notre script sera interprété par **bash**_</p>
<p>`DATE=$(date +%F_%H-%M)`</p>
<p>_Notez la date et l'heure_</p>
<p>`mysqldump -u mantis_user -p'MotDePasse!' bugtracker > /var/backups/mantisbt_$DATE.sql`</p>
<p>_dumper le contenu de notre BDD dans un fichier _</p>
<ul>
<li>`chmod +x backup_mariadb.sh` : mettre les droit d’exécution</li>
<li>`./backup_mariadb.sh` : testez votre script !</li>
<li>`crontab -e` : éditez le gestionnaire de tâche planifiées de Debian</li>
<li>`0 2 * * * /root/backup_mariadb.sh` : tout les jours à 2h</li>
</ul>
<p>Enfin, pour avoir un rappel des mise à jour à chaque login sur le serveur, éditez votre fichier `/root/.bashrc`, et mettez simplement dedans :</p>
<ul>
<li>`apt update -qq && apt list --upgradable`</li>
</ul>
<p>Alors évidemment, il faudrait mettre en place un export de votre sauvegarde, par exemple avec **scp**.</p>
</div>
</details>

Si vous avez été attentif, vous comprenez qu'ici nous avons agit plutôt sur l'**analyse de vulnérabilités** et la **post-exploitation** d'un adversaire.
### Proactif
Ce sont des réactions à mettre en oeuvre face à certains événements. Typiquement, ce sera votre système d'alarme.
 - mettre en place un **fail2ban** (qui repose sur **iptables**, le pare-feu par défaut de Debian), et le configurer pour laisser passer une ip de votre choix, et bannir toutes les autres IP qui échouerai leur connexion **SSH** trois fois
 - Ajouter dans le `bashrc` de root les 3 dernières connexions en tant que root qui sont advenues sur le système.

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Installer **fail2ban**, **iptables** et définissez vous une **IP** en "_whitelist_":</p>
<ul>
<li>`apt install fail2ban`</li>
<li>Ensuite, vous éditez dans la foulée le fichier `/etc/fail2ban/jail.conf`. Cherchez le paramètre `ignoreip =` et mettez à cet endroit l'IP de votre client avec le quel vous accédez à votre serveur.</li>
<li>Enfin, petit bug récent de fail2ban sur Debian, modifier le fichier `/etc/fail2ban/jail.d/defaults-debian.conf` et ajoutez ces lignes derrière `enabled=true` de la catégorie `[sshd]`:</li>
</ul>
<p>`port     = ssh`</p>
<p>`backend  = systemd`</p>
<p>`maxretry = 3`</p>
<p>`bantime  = 1h`</p>
<ul>
<li>et un ptit `systemctl restart fail2ban` pour la route</li>
</ul>
<p>Ensuite, sortez le **banhammer** pour les abus sur **SSH** ! Enfin ... il le fera tout seul. Testez en regardant les logs :</p>
<ul>
<li>`tail -f /var/log/fail2ban.log`</li>
<li>Tentez plusieurs connexion foireuse à SSH, vous devriez voir apparaître quelque chose comme : `[sshd] Ignore VOTRE_IP_DE_CLIENT by ip`</li>
</ul>
<p>Si vous vous êtes enfermée dehors, aller sur le serveur via la console proxmox, et faites :</p>
<ul>
<li>`fail2ban-client status sshd` : voir les bannis</li>
<li>`fail2ban-client set sshd unbanip 172.29.18.249` dé-bannissez vous.</li>
</ul>
<p>Vous avez sans doute remarqué qu'il précise à chaque fois la _jail_ utilisée, ici `[sshd]`. En fait vous pouvez en définir d'autres et les baser sur les logs de votre système ou de votre application...</p>
<p>Aller, Bonus : </p>
<p>	[Fonctionnement de fail2ban](../Appendices/App.05 fail2ban.md)</p>
<p>Ah oui et pour le petit bricolage sur `/root/.bashrc` :</p>
<ul>
<li>`echo "Dernières connexions root :"`</li>
<li>`last -n 3 root`</li>
</ul>
</div>
</details>

Et enfin, vous comprenez qu'ici on agit plutôt sur l'**analyse de vulnérabilités** et l'**exploitation** potentielle d'un adversaire.
## Final
Alors clairement, dans cette section nous ne faisons qu'effleurer la surface des choses : il existe une foule d’autres chantiers à creuser — audit continu des logs, durcissement des permissions et des services, contrôles d'intégrité, segmentation réseau, chiffrement des sauvegardes, WAF/IDS, scans de vulnérabilités réguliers, revues de configuration et tests d’intrusion.

Mais là il nous faudrait des heures, et ce ne serait plus un cursus de développeur mais d'AdminSys spé Cyber.

Enfin, désolé, cette fois-ci, je ne vous demanderais pas de recommencer plusieurs fois cette partie. Je ne compte pas la mettre dans le **TP évalué** final.

Merci de votre patience, d'avoir apprécié ce cours, n'hésitez pas à le partager en dehors de la formation, ou même à vous le mettre en favori pour plus tard.

@Bientôt !



