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
<div class="spoiler-content">
<ul>
<li>Le script <strong>mysql_secure_installation</strong> mérite sa propre page, pour en comprendre le contenu et cela se trouve via <a href="../Appendices/App.01%20mysql_secure_installation.html">ce lien</a>.</li>
<li>On ne créé pas de base de donnée ou de compte particulier, on va utiliser notre compte <strong>root</strong> de <em>mariadb</em> au moment critique.</li>
</ul>
</div>
</details>

### Configurer apache2
Plus tard, nous mettrons nos sources sous `/var/www/mantis/`. Vous aller configurer dès à présent votre serveur pour le servir en _https_ ! Les explication de configuration d'Apache méritent leur propre page. 

Cela se trouve via [ce lien](../Appendices/App.03 Apache.md), et une fois lu, vous allez :
 - Activer les modules nécessaire
 - Définir le fichier de configuration nécessaire en copiant `default-ssl.conf` sous `mantis-ssl.conf`
 - Ajouter une redirection de _http_ vers _https_.

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<ul>
<li><code>a2enmod ssl</code> → active le support SSL (pour le <em>https</em>)</li>
</ul>
<p>Aller dans <code>/etc/apache2/sites-available</code> et faites :</p>
<ul>
<li><code>cp default-ssl.conf mantis-ssl.conf</code></li>
<li><code>cp 000-default.conf mantis-http.conf</code></li>
</ul>
<p>Ensuite on modifie le fichier <code>mantis-http.conf</code> pour qu'il renvoie sur le <em>https</em> : </p>
<p>	`<VirtualHost *:80>`</p>
<p>	<code>ServerName METTEZ_ICI_VOTRE_IP</code></p>
<p>	<code>Redirect permanent / https://METTEZ_ICI_VOTRE_IP/</code></p>
<p>	`</VirtualHost>`</p>
<p>Après cela, on configure le <code>mantis-ssl.conf</code> pour qu'il serve le bon dossier :</p>
<ul>
<li>Définissez <code>DocumentRoot</code> à <code>/var/www/mantis</code></li>
<li>Vérifiez que les lignes suivantes pointent vers les certificats auto-signés :</li>
</ul>
<p>	<code>SSLEngine on</code></p>
<p>	<code>SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem</code></p>
<p>	<code>SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key</code></p>
<p>On <strong>voit</strong> que dans le fichier de configuration d'origine de <em>apache2</em> il est précisé qu'il <strong>faut</strong> installer un package pour avoir des certificats auto-signé, <strong>ssl-cert</strong>.</p>
<ul>
<li><code>apt install ssl-cert</code></li>
</ul>
<p>Nos fichier de configuration sont désormais prêt. Il ne reste plus qu'a désactiver les sites inutiles et mettre les utiles en route... toujours depuis <code>/etc/apache2/sites-available</code>:</p>
<ul>
<li><code>a2dissite *</code></li>
<li><code>a2ensite mantis*</code></li>
</ul>
<p>Et bien sûr ... <code>systemctl reload apache2</code></p>
</div>
</details>

## Final
Pas mal ! Un gros steak, pas vrai ? Courage, on est désormais à deux doigts de la fin… Dans la suite, nous allons déployer nos source.

Cependant, comme je suis un adepte du _drill_ je vous le dit : si vos notes ne vous permettent pas de refaire cette partie… **restaurez votre snapshot et recommencez** !



