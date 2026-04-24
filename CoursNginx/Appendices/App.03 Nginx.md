# Appendice 3: fonctionnement de Nginx
Rendez-vous dans le répertoire de configuration de Nginx : `/etc/nginx`. Faites un `ls` pour voir ce qui s’y trouve. On va détailler chaque élément important.

#### nginx.conf
C’est le fichier de configuration principal de Nginx. Il définit les paramètres globaux (nombre de workers, logs, etc.) et inclut les fichiers de configuration des sites via la directive `include /etc/nginx/sites-enabled/*`.

#### Les répertoires `sites-available` et `sites-enabled`
Comme Apache sur Debian, Nginx utilise un modèle de gestion des sites :
- **sites-available** : Contient les fichiers de configuration de tous vos sites potentiels.
- **sites-enabled** : Contient des **liens symboliques** vers les fichiers de `sites-available` que vous souhaitez réellement activer.

Pour activer un site, on ne tape pas une commande `a2ensite`, on crée manuellement le lien :
`ln -s /etc/nginx/sites-available/mon-site /etc/nginx/sites-enabled/`

#### FastCGI et PHP-FPM
Contrairement à Apache qui peut intégrer PHP via `mod_php`, Nginx ne "parle" pas nativement le PHP. Il utilise le protocole **FastCGI**. 
Nginx reçoit la requête HTTP, voit que c'est du PHP, et "passe le relais" à un service externe appelé **PHP-FPM** (FastCGI Process Manager) via un socket Unix ou un port TCP. PHP-FPM exécute le code et renvoie le résultat à Nginx, qui le renvoie au client.

#### Gestion des logs
Les logs de Nginx se trouvent généralement dans `/var/log/nginx/` :
- `access.log` : Toutes les requêtes reçues.
- `error.log` : Toutes les erreurs rencontrées par le serveur.

<div class="astuce">Après toute modification, testez votre config avec `nginx -t` puis rechargez avec `systemctl reload nginx` !</div>

Instructif non ? Allez, retournez sur [04-Configurer les prerequis](./CoursNginx/Chapitres/04-Configurer%20les%20prerequis.md).
