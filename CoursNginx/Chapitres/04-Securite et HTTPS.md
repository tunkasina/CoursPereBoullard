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

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<ul>
<li>`mysql_secure_installation` (référez-vous à l'appendice du cours si besoin).</li>
</ul>
</div>
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

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Exemple de vhost Nginx complet (HTTP -> HTTPS + PHP-FPM) :</p>
<p>```nginx</p>
<p>server {</p>
<p>    listen 80;</p>
<p>    server_name _;</p>
<p>    return 301 https://$host$request_uri;</p>
<p>}</p>
<p>server {</p>
<p>    listen 443 ssl;</p>
<p>    server_name _;</p>
<p>    root /var/www/mantis;</p>
<p>    index index.php index.html;</p>
<p>    ssl_certificate     /etc/ssl/certs/ssl-cert-snakeoil.pem;</p>
<p>    ssl_certificate_key /etc/ssl/private/ssl-cert-snakeoil.key;</p>
<p>    location / {</p>
<p>        try_files $uri $uri/ =404;</p>
<p>    }</p>
<p>    location ~ \.php$ {</p>
<p>        include snippets/fastcgi-php.conf;</p>
<p>        fastcgi_pass unix:/run/php/php-fpm.sock;</p>
<p>    }</p>
<p>}</p>
<p>```</p>
<p>Commandes :</p>
<p>```bash</p>
<p>apt install ssl-cert</p>
<p>ln -s /etc/nginx/sites-available/mantis /etc/nginx/sites-enabled/</p>
<p>rm /etc/nginx/sites-enabled/default</p>
<p>nginx -t</p>
<p>systemctl reload nginx</p>
<p>```</p>
</div>
</details>

## Final
Vos services sont configurés. Passons au déploiement des sources de Mantis !

[05-Deployer les sources](../../CoursApache/Chapitres/05-Deployer les sources.md)
