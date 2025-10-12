# Consolidation
On appelle **Consolidation** le fait de renforcer notre serveur face aux attaques potentielles.Bien entendu, vous ne savez pas comment s'organise une attaque et heureusement, j'ai pensé à vous.

Allez sur [une attaque informatique](https://tunkasina.github.io/CoursPereBoullard/#/./Appendices/App.04%20attaque%20informatique.md) pour en savoir plus, avant de faire la suite !

<div class="astuce">Faites un snapshot !</div>

## Consignes
Vous n'aurez pas le temps de tout faire. Vous devrez choisir parmi les rubriques suivantes, ce qui vous intéresse plus ... Vous ne serez pas évalué dessus, bien sûr !

### Passif
Cela consiste à limiter ce que l'on offre de manière passive, aux potentiels agresseurs. Voyez cela comme le fait de ne pas laisser la clé sur la serrure, mais aussi comme le fait d'éviter de mettre une grosse flèche rouge vers votre coffre-fort, ou de laisser la porte du jardin ouverte.

#### Post installation
 - Appliquez les instructions post-installation de **Mantis** 
 - Contrôler les droits utiles et nécessaire sur `/var/www/mantis`

[spoiler]
A plusieurs endroits **Mantis** nous indique des choses à faire :
 - _**Attention :** vous devriez désactiver le compte « administrator » par défaut ou changer son mot de passe._
 - _**Attention :** le répertoire « admin » par défaut devrait être supprimé ou son accès devrait être restreint._

Puise que c'est le logiciel qui le dit, faites ! Et tant que vous avez le nez dans votre _shell_ pensez à modifier les droits que l'on avait un peu trop ouvert dans `/var/www/mantis/config`, vous vous souvenez ? Allez dans `mantis`
 - `chmod -R 750 config`
 - `find . -type f -print0 | xargs -0 chmod 640`

Et comme on ne supprime rien sans avoir la possibilité de le restaurer :
 - `mv admin ~/admin.old`

[/spoiler]

#### Discrétion
 - Obtenez un 404 et trouvez les infos en trop
 - Interrogez votre serveur pour obtenir les **HEADERS** renvoyés
 - Diminuez la signature d'**Apache**

[spoiler]
 - Essayer `http://[VOTRE_IP]/test` : vous obtenez des choses comme _Apache/2.4.65 (Debian) OpenSSL/3.0.17 Server at 172.22.69.238 Port 443_
 - Faites **F12** pour avoir vos outils de dev', et cherchez dans **Réseau**. Prenez la première requête **GET** et regardez les **en-têtes**. Vous devriez trouvez quelque chose comme _Server: Apache/2.4.65 (Debian) OpenSSL/3.0.17_

Ce genre d'info, c'est le exactement ce que vous ne _voulez pas_ montrer. Pour empêcher ce comportement, éditez `/etc/apache2/conf-available/security.conf`, et passez les paramètres `ServerTokens` à `Prod` et `ServerSignature` à `Off`.

Dans le temps, **PHP** affichait ses infos de version à chaque plantage. Soyez méfiant, si vous voyez un numéro de version apparaître lors de vos session de code, c'est qu'il y a une option quelque part à désactiver !

[/spoiler]

#### Seulement l'utile et le nécessaire
 - Vérifier vos port ouverts vers l’extérieur

[spoiler]
Heureusement vous n'avez rien d'ouvert ! Voici comment le vérifier:
 - `ss -tul`

Vous verrez toutes les écoutes (`l`) tcp (`t`) et udp (`u`). Et dedans on constate que **SSH** écoute sur toutes les interface IPv4 (`0.0.0.0`), **mariadb** seulement la boucle locale (`127.0.0.1`), et **Apache** écoute toutes les interfaces possibles (`*:http`, `*:https`) (_oui Apache est très à l'écoute_ ).

[/spoiler]

Si vous avez été attentif, vous comprendrez qu'en étant soucieux de rester discret, et de n'exposer que l'utile et le nécessaire, nous agissons sur l'étape de **reconnaissance** et l'**exploitation** d'un adversaire.
### Actif
Cela consiste à mener des actions de protection, afin d'augmenter la résilience de notre système. C'est la maintenance de notre système, et des bonnes pratiques, de bonne santé.
 - Mettre en place un script de sauvegarde de la base de donnée
 - Mettre en place un script de vérification de mise à jours dans le `.bashrc` (qui s'affichera donc dès que vous vous connectez sur le serveur)

[spoiler]
Un simple script de sauvegarde sera déjà un bon début ! Les admins prévoyants en mettent souvent quelques-un à divers étages ... 
Directement dans le home de root, créez un script **shell** :
 - `nano backup_mariadb.sh`

Et mettre dedans (avec explications ligne à ligne !):
`#!/bin/bash`
_signifie que notre script sera interprété par **bash**_
`DATE=$(date +%F_%H-%M)`
_Notez la date et l'heure_
`mysqldump -u mantis_user -p'MotDePasse!' bugtracker > /var/backups/mantisbt_$DATE.sql`
_dumper le contenu de notre BDD dans un fichier _
 - `chmod +x backup_mariadb.sh` : mettre les droit d’exécution
 - `./backup_mariadb.sh` : testez votre script !
 - `crontab -e` : éditez le gestionnaire de tâche planifiées de Debian
 - `0 2 * * * /root/backup_mariadb.sh` : tout les jours à 2h

Enfin, pour avoir un rappel des mise à jour à chaque login sur le serveur, éditez votre fichier `bashrc`, et mettez simplement dedans :
 - `apt update -qq && apt list --upgradable`

Alors évidemment, il faudrait mettre en place un export de votre sauvegarde, par exemple avec **scp**.

[/spoiler]

Si vous avez été attentif, vous comprenez qu'ici nous avons agit plutôt sur l'**analyse de vulnérabilités** et la **post-exploitation** d'un adversaire.
### Proactif
Ce sont des réactions à mettre en oeuvre face à certains événements. Typiquement, ce sera votre système d'alarme.
 - mettre en place un **fail2ban** (qui repose sur **iptables**), et le configurer pour laisser passer une ip de votre choix, et bannir toutes les autres IP qui échouerai leur connexion **SSH** trois fois
 - en se basant sur les logs d'**Apache**, bannissez également les gens qui échouent à se connecter sur l'interface trois fois.
 - Ajouter dans le `bashrc` de root les 3 dernières connexions en tant que root qui sont advenues sur le système.

[spoiler]
Installer **fail2ban**, **iptables** et définissez vous une **IP** en "_whitelist_":
 - `apt install fail2ban`
 - Ensuite, vous éditez dans la foulée le fichier `/etc/fail2ban/jail.conf`. Cherchez le paramètre `ignoreip =` et mettez à cet endroit l'IP de votre client avec le quel vous accédez à votre serveur.
 - Enfin, petit bug récent de fail2ban sur Debian, modifier le fichier `/etc/fail2ban/jail.d/defaults-debian.conf` et ajoutez ces lignes derrière `enabled=true` de la catégorie `[sshd]`:

`port     = ssh`
`backend  = systemd`
`maxretry = 3`
`bantime  = 1h`
 - et un ptit `systemctl restart fail2ban` pour la route

Ensuite, sortez le **banhammer** pour les abus sur **SSH** ! Enfin ... il le fera tout seul. Testez en regardant les logs :
 - `tail -f /var/log/fail2ban.log`
 - Tentez plusieurs connexion foireuse à SSH, vous devriez voir apparaître quelque chose comme : `[sshd] Ignore VOTRE_IP_DE_CLIENT by ip`

Si vous vous êtes enfermée dehors, aller sur le serveur via la console proxmox, et faites :
 - `fail2ban-client status sshd` : voir les bannis
 - `fail2ban-client set sshd unbanip 172.29.18.249` dé-bannissez vous.

Vous avez sans doute remarqué qu'il précise à chaque fois la _jail_ utilisée, ici `[sshd]`.

[/spoiler]

Et enfin, vous comprenez qu'ici on agit plutôt sur l'**analyse de vulnérabilités** et l'**exploitation** potentielle d'un adversaire.
## Final
Et voilà ! Désolé, cette fois-ci, je ne vous demanderais pas de recommencer plusieurs fois cette partie. Je ne compte pas la mettre dans le **TP évalué** final.

Merci de votre patience, d'avoir apprécié ce cours, n'hésitez pas à le partager en dehors de la formation, ou même à vous le mettre en favori pour plus tard.

@Bientôt !



