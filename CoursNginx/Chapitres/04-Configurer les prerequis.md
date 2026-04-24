# Configurer les prérequis
On va maintenant configurer nos services : **Nginx** et **MariaDB**.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Configurer MariaDB
 - Configurer _mariadb_ avec le script _mysql_secure_installation_.

[spoiler]
 - Le script **mysql_secure_installation** sécurise l'accès root et supprime les comptes anonymes. Voir [cet appendice](https://tunkasina.github.io/CoursPereBoullard/#/./CoursApache/Appendices/App.01%20mysql_secure_installation.md) (valable pour Apache et Nginx).
[/spoiler]

### Configurer Nginx
Nous allons configurer Nginx pour servir notre application dans `/var/www/mantis/` en HTTPS.
Lisez d'abord [l'appendice sur Nginx](CoursNginx/Appendices/App.03%20Nginx.md), puis :
 - Créer un fichier de configuration pour le site.
 - Configurer la redirection HTTP $\rightarrow$ HTTPS.
 - Configurer le support SSL avec des certificats auto-signés.
 - Lier Nginx à PHP-FPM.

[spoiler]
1. **Certificats auto-signés** :
   `apt install ssl-cert`

2. **Configuration du site** :
   Allez dans `/etc/nginx/sites-available/` et créez `mantis.conf` :

```nginx
server {
    listen 80;
    server_name VOTRE_IP;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl;
    server_name VOTRE_IP;

    ssl_certificate /etc/ssl/certs/ssl-cert-snakeoil.pem;
    ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;

    root /var/www/mantis;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php-fpm.sock; # Vérifiez le chemin du socket !
    }
}
```

3. **Activation** :
   - `rm /etc/nginx/sites-enabled/default` (on enlève le site par défaut)
   - `ln -s /etc/nginx/sites-available/mantis.conf /etc/nginx/sites-enabled/`
   - `nginx -t` (pour vérifier la syntaxe)
   - `systemctl reload nginx`
[/spoiler]

## Final
C'est fait ! On a un serveur web sécurisé et prêt à parler à PHP. Prochaine étape : le déploiement des sources.

[05-Mise en œuvre des sources](./05-Deployer%20les%20sources.md)
