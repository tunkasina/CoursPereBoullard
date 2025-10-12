# Configurer les prérequis
Enfin ! On va réellement configurer les deux services principaux de notre stack: _apache2_ et _mariadb_. Rien de bien compliqué, mais il faut rester attentif aux différents détails.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tout les moyens nécessaires blablabla... Vous connaissez la chanson non, depuis le temps ?!
### Configurer mariadb
 - Configurer _mariadb_ avec le script _mysql_secure_installation_ (attention cherchez à comprendre ce que vous faites)
 - Créer une base de donnee _mantis_db_, un utilisateur pour cette base de donnée _mantis_db_admin_, et donnez lui tout les droits sur cette base de donnée.

[spoiler]
 - Le script **mysql_secure_installation** mérite sa propre page, pour en comprendre le contenu et cela se trouve via [ce lien](https://tunkasina.github.io/CoursPereBoullard/#/./Appendices/App.01%20mysql_secure_installation.md).
 - On ne créé pas de base de donnée ou de compte particulier, on va utiliser notre compte **root** de _mariadb_ au moment critique.
[/spoiler]

### Configurer apache2
Plus tard, nous mettrons nos sources sous `/var/www/mantis/`. Vous aller configurer dès à présent votre serveur pour le servir en _https_ !
La configuration d'Apache mérite sa propre page :
<div class="astuce">
[ce lien](https://tunkasina.github.io/CoursPereBoullard/#/./Appendices/App.03%20Apache.md)
</div>
Les explication de configuration d'Apache méritent leur propre page. Cela se trouve via . Une fois lu, vous allez :
 - Activer les modules nécessaire
 - Définir le fichier de configuration nécessaire en copiant `default-ssl.conf` sous `mantis-ssl.conf`
 - Ajouter une redirection de _http_ vers _https_.

[spoiler]
 - `a2enmod ssl` → active le support SSL (pour le _https_)

Aller dans `/etc/apache2/sites-available` et faites :
 - `cp default-ssl.conf mantis-ssl.conf`
 - `cp 000-default.conf mantis-http.conf`

Ensuite on modifie le fichier `mantis-http.conf` pour qu'il renvoie sur le _https_ : 
	`<VirtualHost *:80>`
	`ServerName METTEZ_ICI_VOTRE_IP`
	`Redirect permanent / https://METTEZ_ICI_VOTRE_IP/`
	`</VirtualHost>`

Après cela, on configure le `mantis-ssl.conf` pour qu'il serve le bon dossier :
 - Définissez `DocumentRoot` à `/var/www/mantis`
 - Vérifiez que les lignes suivantes pointent vers les certificats auto-signés :

	`SSLEngine on`
	`SSLCertificateFile    /etc/ssl/certs/ssl-cert-snakeoil.pem`
	`SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key`

On **voit** que dans le fichier de configuration d'origine de _apache2_ il est précisé qu'il **faut** installer un package pour avoir des certificats auto-signé, **ssl-cert**.
 - `apt install ssl-cert`

Nos fichier de configuration sont désormais prêt. Il ne reste plus qu'a désactiver les sites inutiles et mettre les utiles en route... toujours depuis `/etc/apache2/sites-available`:
 - `a2dissite *`
 - `a2ensite mantis*`

Et bien sûr ... `systemctl reload apache2`
[/spoiler]

## Final
Pas mal ! Un gros steak, pas vrai ? Courage, on est désormais à deux doigts de la fin… Dans la suite, nous allons déployer nos source.

Cependant, comme je suis un adepte du _drill_ je vous le dit : si vos notes ne vous permettent pas de refaire cette partie… **restaurez votre snapshot et recommencez** !