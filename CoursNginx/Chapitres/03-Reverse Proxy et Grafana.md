# Reverse Proxy et Grafana
Enfin ! On va installer Grafana et le mettre derrière Nginx en reverse proxy. C'est là que Nginx montre sa vraie puissance : il ne se contente pas de servir des fichiers statiques, il agit comme une passerelle intelligente.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires... Vous connaissez la chanson non, depuis le temps ?

### Installer Grafana
Grafana n'est pas dans les dépôts Debian par défaut. Il va falloir ajouter le dépôt officiel.
 - Installez Grafana sur votre serveur
 - Vérifiez qu'il fonctionne et écoute sur le port 3000
 - Accédez-y depuis votre navigateur pour confirmer

[spoiler]
Ajout du dépôt et installation :
```bash
apt install -y software-properties-common wget
wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" > /etc/apt/sources.list.d/grafana.list
apt update && apt install -y grafana
systemctl enable --now grafana-server
```

Vérification :
 - `ss -tlnp | grep 3000`
 - `curl -I http://localhost:3000` (doit renvoyer un statut 302 vers /login)
 - Allez sur `http://[IP_VM]:3000` (identifiants par défaut : `admin` / `admin`).
[/spoiler]

### Sécuriser Grafana
Par défaut, Grafana écoute sur `0.0.0.0:3000`, c'est-à-dire accessible directement depuis l'extérieur. Ce n'est pas ce qu'on veut : Nginx doit être le seul point d'entrée.
 - Modifiez la configuration de Grafana pour qu'il écoute uniquement sur la boucle locale (`127.0.0.1`)
 - Redémarrez Grafana
 - Vérifiez que le port 3000 n'est plus accessible depuis l'extérieur

[spoiler]
Dans `/etc/grafana/grafana.ini`, section `[server]` :
```ini
[server]
http_addr = 127.0.0.1
http_port = 3000
```
Puis :
 - `systemctl restart grafana-server`
 - `ss -tlnp | grep 3000` (doit montrer `127.0.0.1:3000`)
[/spoiler]

### Configurer Nginx en reverse proxy
Maintenant qu'il est planqué sur localhost, il faut que Nginx fasse le pont.
 - Ajoutez un bloc `upstream` pointant vers `127.0.0.1:3000`
 - Dans votre serveur virtuel, faites pointer la location `/` vers cet upstream avec `proxy_pass`
 - Transmettez les en-têtes nécessaires (`Host`, `X-Real-IP`, `X-Forwarded-For`, `X-Forwarded-Proto`)
 - Ajoutez une location pour les WebSockets (`/api/live/`)
 - Testez et rechargez

[spoiler]
Dans votre vhost `/etc/nginx/sites-available/grafana-project` :
```nginx
upstream grafana_backend {
    server 127.0.0.1:3000;
}

server {
    listen 80;
    server_name _;

    root /var/www/grafana-project;

    location / {
        proxy_pass http://grafana_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
    }

    location /api/live/ {
        proxy_pass http://grafana_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
Testez et appliquez : `nginx -t && systemctl reload nginx`
[/spoiler]

### Configurer Grafana pour le reverse proxy
Grafana a besoin de savoir qu'il est derrière un reverse proxy pour générer de bonnes URL.
 - Modifiez `/etc/grafana/grafana.ini` pour renseigner `domain` et `root_url`
 - Redémarrez Grafana

[spoiler]
Dans `/etc/grafana/grafana.ini` :
```ini
[server]
domain = [IP_VOTRE_VM]
root_url = http://[IP_VOTRE_VM]/
```
Puis `systemctl restart grafana-server`.
[/spoiler]

## Final
Grafana est fonctionnel derrière Nginx. Vous avez un reverse proxy qui centralise l'accès. Mais tout passe en clair (HTTP). 

Dans la suite, on va ajouter le HTTPS et la sécurité.

[04-Securite et HTTPS](./04-Securite%20et%20HTTPS.md)
