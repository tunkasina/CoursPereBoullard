Enfin ! On va réellement configurer les deux services principaux de notre stack: _apache2_ et _mariadb_. Rien de bien compliqué, mais il faut rester attentif aux différents détails.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tout les moyens nécessaires blablabla... Vous connaissez la chanson non, depuis le temps ?!
### Configurer mariadb
 - Configurer _mariadb_ avec le script _mysql_secure_installation_ (attention cherchez à comprendre ce que vous faites)
 - Créer une base de donnee _mantis_db_, un utilisateur pour cette base de donnée _mantis_db_admin_, et donnez lui tout les droits sur cette base de donnée.

[spoiler]
 - Le script **mysql_secure_installation** mérite sa propre page, pour en comprendre le contenu et cela se trouve via [ce lien](./CoursApache/Chapitres/App.01%20mysql_secure_installation.md).
 - On ne créé pas compte particulier, on va utiliser notre compte **root** de _mariadb_ au moment critique.
[/spoiler]

### Configurer apache2
 - Plus tard, nous mettrons les sources accessible sous `/var/www/mantis/`. Configurez dès à présent votre serveur pour servir ce _répertoire_ en _https_.
 - Ajouter une redirection de _http_ vers _https_.
 

[spoiler]
	Les explication de configuration d'Apache méritent leur propre pa
[/spoiler]

## Final
On est désormais à deux doigts de la fin… Dans la suite, nous allons déployer nos source.

Cependant, comme je suis un adepte du _drill_ je vous le dit : si vos notes ne vous permettent pas de refaire cette partie… **restaurez votre snapshot et recommencez** !