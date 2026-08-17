---
title: "Configurer les prérequis"
parent: "TP Apache"
nav_order: 4
---
# Configurer les prérequis
Enfin ! On va réellement configurer les deux services principaux de notre stack: _apache2_ et _mariadb_. Rien de bien compliqué, mais il faut rester attentif aux différents détails.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tout les moyens nécessaires blablabla... Vous connaissez la chanson non, depuis le temps ?!
### Configurer mariadb
 - Configurer _mariadb_ avec le script _mysql_secure_installation_ (attention cherchez à comprendre ce que vous faites)

<details class="spoiler">
<summary>Solution / Indice</summary>
<ul>
<li>Le script **mysql_secure_installation** mérite sa propre page, pour en comprendre le contenu et cela se trouve via [ce lien](../Appendices/App.01%20mysql_secure_installation.html).</li>
<li>On ne créé pas de base de donnée ou de compte particulier, on va utiliser notre compte **root** de _mariadb_ au moment critique.</li>
</ul>
</details>

### Configurer apache2
Plus tard, nous mettrons nos sources sous `/var/www/mantis/`. Vous aller configurer dès à présent votre serveur pour le servir en _https_ ! Les explication de configuration d'Apache méritent leur propre page. 

Cela se trouve via [ce lien](../Appendices/App.03%20Apache.html), et une fois lu, vous allez :
 - Activer les modules nécessaire
 - Définir le fichier de configuration nécessaire en copiant `default-ssl.conf` sous `mantis-ssl.conf`
 - Ajouter une redirection de _http_ vers _https_.

<details class="spoiler">
<summary>Solution / Indice</summary>
<ul>
<li>`a2enmod ssl` → active le support SSL (pour le _https_)</li>
</ul>
<p>Aller dans `/etc/apache2/sites-available` et faites :</p>
<ul>
<li>`cp default-ssl.conf mantis-ssl.conf`</li>
<li>`cp 000-default.conf mantis-http.conf`</li>
</ul>
<p>Ensuite on modifie le fichier `mantis-http.conf` pour qu'il renvoie sur le _https_ : </p>
<p>	`<VirtualHost *:80>`</p>
<p>	`ServerName METTEZ_ICI_VOTRE_IP`</p>
<p>	`Redirect permanent / https://METTEZ_ICI_VOTRE_IP/`</p>
<p>	`</VirtualHost>`</p>
<p>Après cela, on configure le `mantis-ssl.conf` pour qu'il serve le bon dossier :</p>
<ul>
<li>Définissez `DocumentRoot` à `/var/www/mantis`</li>
<li>Vérifiez que les lignes suivantes pointent vers les certificats auto-signés :</li>
</ul>
<p>	`SSLEngine on`</p>
<p>	`SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem`</p>
<p>	`SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key`</p>
<p>On **voit** que dans le fichier de configuration d'origine de _apache2_ il est précisé qu'il **faut** installer un package pour avoir des certificats auto-signé, **ssl-cert**.</p>
<ul>
<li>`apt install ssl-cert`</li>
</ul>
<p>Nos fichier de configuration sont désormais prêt. Il ne reste plus qu'a désactiver les sites inutiles et mettre les utiles en route... toujours depuis `/etc/apache2/sites-available`:</p>
<ul>
<li>`a2dissite *`</li>
<li>`a2ensite mantis*`</li>
</ul>
<p>Et bien sûr ... `systemctl reload apache2`</p>
</details>

## Final
Pas mal ! Un gros steak, pas vrai ? Courage, on est désormais à deux doigts de la fin… Dans la suite, nous allons déployer nos source.

Cependant, comme je suis un adepte du _drill_ je vous le dit : si vos notes ne vous permettent pas de refaire cette partie… **restaurez votre snapshot et recommencez** !



