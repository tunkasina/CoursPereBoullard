# Reverse Proxy et Grafana
Enfin ! On va installer Grafana et le mettre derriere NGinx en reverse proxy. C'est la que NGinx montre sa vraie puissance : il ne se contente pas de servir des fichiers statiques, il peut agir comme une passerelle intelligente vers d'autres services.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous meme et par tous les moyens necessaires... Vous connaissez la chanson non, depuis le temps ?

### Installer Grafana
Grafana n'est pas dans les depots Debian par defaut. Il va falloir ajouter le depot officiel.
 - Installez Grafana sur votre serveur
 - Verifiez qu'il fonctionne et ecoute sur le port 3000
 - Accedez-y depuis votre navigateur pour confirmer

[spoiler]
```bash
apt install -y software-properties-common wget
wget -q -O /usr/share/keyrings/grafana.key \
  https://apt.grafana.com/gpg.key
echo "deb [signed-by=/usr/share/keyrings/grafana.key] \
  https://apt.grafana.com stable main" \
  > /etc/apt/sources.list.d/grafana.list

apt update && apt install -y grafana
systemctl enable grafana-server
systemctl start grafana-server
```

Verification :
```bash
ss -tlnp | grep 3000
curl -I http://localhost:3000
# Doit repondre 302 (redirection vers /login)
```

Allez sur `http://[IP_VM]:3000` dans votre navigateur. Vous devriez voir la page de connexion Grafana. L'identifiant par defaut est `admin` / `admin`. Changez le mot de passe quand on vous le demande.
[/spoiler]

### Securiser Grafana
Par defaut, Grafana ecoute sur `0.0.0.0:3000`, c'est-a-dire accessible depuis n'importe quelle machine du reseau. Ce n'est pas ce qu'on veut : on veut que NGinx soit le seul point d'entree.
 - Modifiez la configuration de Grafana pour qu'il ecoute uniquement sur `127.0.0.1` (localhost)
 - Redemarrez Grafana
 - Verifiez que le port 3000 n'est plus accessible depuis l'exterieur

[spoiler]
Editez `/etc/grafana/grafana.ini` et modifiez (ou ajoutez) dans la section `[server]` :
```ini
[server]
http_addr = 127.0.0.1
http_port = 3000
```

Puis :
```bash
systemctl restart grafana-server
ss -tlnp | grep 3000
# Doit montrer 127.0.0.1:3000, pas 0.0.0.0:3000
```

Essayez d'acceder a `http://[IP_VM]:3000` depuis le navigateur... ca ne marche plus. Normal, Grafana n'est plus accessible directement. C'est voulu.
[/spoiler]

### Configurer NGinx en reverse proxy
Maintenant qu'il est planque sur localhost, il faut que NGinx fasse le pont entre le monde exterieur et Grafana.
 - Ajoutez un bloc `upstream` pointant vers `127.0.0.1:3000`
 - Dans le server block existant, faites pointer la location `/` vers cet upstream avec `proxy_pass`
 - N'oubliez pas les headers de forwarding (`Host`, `X-Real-IP`, `X-Forwarded-For`, `X-Forwarded-Proto`)
 - Ajoutez une location speciale pour les WebSockets (`/api/live/`) avec support de l'upgrade
 - Testez et rechargez

[spoiler]
Votre fichier `/etc/nginx/sites-available/grafana-project` devrait maintenant ressembler a ca :

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
        proxy_buffering off;
        proxy_http_version 1.1;
    }

    location /api/live/ {
        proxy_pass http://grafana_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /static/ {
        expires 7d;
        add_header Cache-Control "public";
    }

    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location ~ /\. {
        deny all;
        return 403;
    }
}
```

```bash
nginx -t && systemctl reload nginx
```

Maintenant, `http://[IP_VM]/` devrait vous afficher Grafana, sans avoir a mettre le port 3000. Magique, non ?
[/spoiler]

### Configurer Grafana pour le reverse proxy
Grafana a besoin de savoir qu'il est derriere un reverse proxy, pour generer les bonnes URLs.
 - Modifiez le fichier `/etc/grafana/grafana.ini` pour renseigner le `domain` et le `root_url`
 - Redemarrez Grafana

[spoiler]
```ini
[server]
domain = [IP_VOTRE_VM]
root_url = http://[IP_VOTRE_VM]/
```

```bash
systemctl restart grafana-server
```

Sans cette config, Grafana pourrait generer des redirections vers `localhost:3000` au lieu de votre IP. Et la, personne ne comprend rien.
[/spoiler]

### Voir les IPs dans les logs
Un des interets du proxy est de transmettre l'IP du client. Verifions que ca marche.
 - Consultez les logs NGinx (`/var/log/nginx/grafana-access.log`)
 - Faites quelques requetes et observez les IPs
 - Si vous avez acces aux logs Grafana, regardez si l'IP reelle apparait

[spoiler]
```bash
tail -f /var/log/nginx/grafana-access.log
```

Vous devriez voir les requetes avec la bonne IP. Si ce n'est pas le cas, verifiez vos headers `proxy_set_header X-Real-IP $remote_addr;`.

Pour les logs Grafana : `cat /var/log/grafana/grafana.log | grep -i "X-Real-IP"`
[/spoiler]

## Final
Grafana est fonctionnel derriere NGinx. Vous avez un reverse-oproxy qui ajoute une couche de securite et de flexibilite. Mais pour l'instant c'est en HTTP, pas genial pour un outil de monitoring expose sur le web...

Dans la suite, on va ajouter le HTTPS et la securite.

[04-Securite et HTTPS](./04-Securite%20et%20HTTPS.md)
