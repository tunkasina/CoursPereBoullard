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
<p>A plusieurs endroits <strong>Mantis</strong> nous indique des choses à faire :</p>
<ul>
<li><em><strong>Attention :</strong> vous devriez désactiver le compte « administrator » par défaut ou changer son mot de passe.</em></li>
<li><em><strong>Attention :</strong> le répertoire « admin » par défaut devrait être supprimé ou son accès devrait être restreint.</em></li>
</ul>
<p>Puise que c'est le logiciel qui le dit, faites ! Et tant que vous avez le nez dans votre <em>shell</em> pensez à modifier les droits que l'on avait un peu trop ouvert dans <code>/var/www/mantis/config</code>, vous vous souvenez ? Allez dans <code>mantis</code></p>
<ul>
<li><code>chmod -R 750 config</code></li>
<li><code>find . -type f -print0 | xargs -0 chmod 640</code></li>
</ul>
<p>Et comme on ne supprime rien sans avoir la possibilité de le restaurer :</p>
<ul>
<li><code>mv admin ~/admin.old</code></li>
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
<li>Essayer <code>http://[VOTRE_IP]/test</code> : vous obtenez des choses comme <em>Apache/2.4.65 (Debian) OpenSSL/3.0.17 Server at 172.22.69.238 Port 443</em></li>
<li>Faites <strong>F12</strong> pour avoir vos outils de dev', et cherchez dans <strong>Réseau</strong>. Prenez la première requête <strong>GET</strong> et regardez les <strong>en-têtes</strong>. Vous devriez trouvez quelque chose comme <em>Server: Apache/2.4.65 (Debian) OpenSSL/3.0.17</em></li>
</ul>
<p>Ce genre d'info, c'est le exactement ce que vous ne <em>voulez pas</em> montrer. Pour empêcher ce comportement, éditez <code>/etc/apache2/conf-available/security.conf</code>, et passez les paramètres <code>ServerTokens</code> à <code>Prod</code> et <code>ServerSignature</code> à <code>Off</code>.</p>
<p>Dans le temps, <strong>PHP</strong> affichait ses infos de version à chaque plantage. Soyez méfiant, si vous voyez un numéro de version apparaître lors de vos session de code, c'est qu'il y a une option quelque part à désactiver !</p>
</div>
</details>

#### Seulement l'utile et le nécessaire
 - Vérifier vos port ouverts vers l’extérieur

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Heureusement vous n'avez rien d'ouvert ! Voici comment le vérifier:</p>
<ul>
<li><code>ss -tul</code></li>
</ul>
<p>Vous verrez toutes les écoutes (<code>l</code>) tcp (<code>t</code>) et udp (<code>u</code>). Et dedans on constate que <strong>SSH</strong> écoute sur toutes les interface IPv4 (<code>0.0.0.0</code>), <strong>mariadb</strong> seulement la boucle locale (<code>127.0.0.1</code>), et <strong>Apache</strong> écoute toutes les interfaces possibles (<code>*:http</code>, <code>*:https</code>) (<em>oui Apache est très à l'écoute</em> ).</p>
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
<p>Directement dans le home de root, créez un script <strong>shell</strong> :</p>
<ul>
<li><code>nano backup_mariadb.sh</code></li>
</ul>
<p>Et mettre dedans (avec explications ligne à ligne !):</p>
<p><code>#!/bin/bash</code></p>
<p><em>signifie que notre script sera interprété par <strong>bash</strong></em></p>
<p><code>DATE=$(date +%F_%H-%M)</code></p>
<p><em>Notez la date et l'heure</em></p>
<p><code>mysqldump -u mantis_user -p'MotDePasse!' bugtracker > /var/backups/mantisbt_$DATE.sql</code></p>
<p><em>dumper le contenu de notre BDD dans un fichier </em></p>
<ul>
<li><code>chmod +x backup_mariadb.sh</code> : mettre les droit d’exécution</li>
<li><code>./backup_mariadb.sh</code> : testez votre script !</li>
<li><code>crontab -e</code> : éditez le gestionnaire de tâche planifiées de Debian</li>
<li><code>0 2 * * * /root/backup_mariadb.sh</code> : tout les jours à 2h</li>
</ul>
<p>Enfin, pour avoir un rappel des mise à jour à chaque login sur le serveur, éditez votre fichier <code>/root/.bashrc</code>, et mettez simplement dedans :</p>
<ul>
<li><code>apt update -qq && apt list --upgradable</code></li>
</ul>
<p>Alors évidemment, il faudrait mettre en place un export de votre sauvegarde, par exemple avec <strong>scp</strong>.</p>
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
<p>Installer <strong>fail2ban</strong>, <strong>iptables</strong> et définissez vous une <strong>IP</strong> en "<em>whitelist</em>":</p>
<ul>
<li><code>apt install fail2ban</code></li>
<li>Ensuite, vous éditez dans la foulée le fichier <code>/etc/fail2ban/jail.conf</code>. Cherchez le paramètre <code>ignoreip =</code> et mettez à cet endroit l'IP de votre client avec le quel vous accédez à votre serveur.</li>
<li>Enfin, petit bug récent de fail2ban sur Debian, modifier le fichier <code>/etc/fail2ban/jail.d/defaults-debian.conf</code> et ajoutez ces lignes derrière <code>enabled=true</code> de la catégorie <code>[sshd]</code>:</li>
</ul>
<p><code>port     = ssh</code></p>
<p><code>backend  = systemd</code></p>
<p><code>maxretry = 3</code></p>
<p><code>bantime  = 1h</code></p>
<ul>
<li>et un ptit <code>systemctl restart fail2ban</code> pour la route</li>
</ul>
<p>Ensuite, sortez le <strong>banhammer</strong> pour les abus sur <strong>SSH</strong> ! Enfin ... il le fera tout seul. Testez en regardant les logs :</p>
<ul>
<li><code>tail -f /var/log/fail2ban.log</code></li>
<li>Tentez plusieurs connexion foireuse à SSH, vous devriez voir apparaître quelque chose comme : <code>[sshd] Ignore VOTRE_IP_DE_CLIENT by ip</code></li>
</ul>
<p>Si vous vous êtes enfermée dehors, aller sur le serveur via la console proxmox, et faites :</p>
<ul>
<li><code>fail2ban-client status sshd</code> : voir les bannis</li>
<li><code>fail2ban-client set sshd unbanip 172.29.18.249</code> dé-bannissez vous.</li>
</ul>
<p>Vous avez sans doute remarqué qu'il précise à chaque fois la <em>jail</em> utilisée, ici <code>[sshd]</code>. En fait vous pouvez en définir d'autres et les baser sur les logs de votre système ou de votre application...</p>
<p>Aller, Bonus : </p>
<p>	<a href="../Appendices/App.05%20fail2ban.html">Fonctionnement de fail2ban</a></p>
<p>Ah oui et pour le petit bricolage sur <code>/root/.bashrc</code> :</p>
<ul>
<li><code>echo "Dernières connexions root :"</code></li>
<li><code>last -n 3 root</code></li>
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



