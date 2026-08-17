---
title: "Configurer les prérequis"
parent: "TP Nginx"
nav_order: 4
---
# Configurer les prérequis
Nous allons configurer MariaDB et préparer notre serveur virtuel Nginx pour accueillir Mantis en HTTPS.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Configurer MariaDB
 - Sécurisez votre installation MariaDB via le script dédié.

<details class="spoiler"><summary>Solution / Indice</summary>
 - `mysql_secure_installation` (référez-vous à l'appendice du cours si besoin).
</details>

### Configurer Nginx pour Mantis et HTTPS
Contrairement à Apache qui gère les certificats par défaut, Nginx nécessite une configuration explicite du bloc SSL et du passage à PHP-FPM.
 - Créez un certificat auto-signé ou utilisez le paquet `ssl-cert`.
 - Configurez votre VirtualHost Nginx dans `/etc/nginx/sites-available/mantis` pour :
   - Rediriger le HTTP vers le HTTPS.
   - Écouter en HTTPS sur le port 443.
   - Définir la racine (`root /var/www/mantis;`).
   - Activer l'interprétation PHP via PHP-FPM (`location ~ \.php$`).
 - Activez votre site via un lien symbolique et désactivez le site par défaut.
 - Testez la syntaxe (`nginx -t`) et rechargez (`systemctl reload nginx`).

<details class="spoiler"><summary>Solution / Indice</summary>
Exemple de vhost Nginx complet (HTTP -> HTTPS + PHP-FPM) :
```nginx
server {
    listen 80;
    server_name _;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name _;

    root /var/www/mantis;
    index index.php index.html;

    ssl_certificate     /etc/ssl/certs/ssl-cert-snakeoil.pem;
    ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php-fpm.sock;
    }
}
```
Commandes :
```bash
apt install ssl-cert
ln -s /etc/nginx/sites-available/mantis /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default
nginx -t
systemctl reload nginx
```
</details>

## Final
Vos services sont configurés. Passons au déploiement des sources de Mantis !

[05-Deployer les sources](./05-Deployer%20les%20sources.md)
