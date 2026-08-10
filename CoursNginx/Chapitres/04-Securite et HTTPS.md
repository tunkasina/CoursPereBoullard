# Securite et HTTPS
Votre Grafana est accessible en HTTP, en clair. N'importe qui sur le reseau peut voir les mots de passe, les dashboards, et les donnees potentiellement sensibles. Pas ideal pour un outil de monitoring d'entreprise, vous en conviendrez.

On va corriger tout ca : HTTPS, rate limiting, headers de securite, et un peu de durcissement.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous meme et par tous les moyens necessaires...

### Creer un certificat auto-signe
Pour du HTTPS, il faut un certificat. En production, on utilise Let's Encrypt. Pour notre TP, un certificat auto-signe suffira.
 - Creez un repertoire `/etc/nginx/ssl/`
 - Generer une cle RSA 2048 bits et un certificat auto-signe avec OpenSSL

[spoiler]
```bash
mkdir -p /etc/nginx/ssl
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout /etc/nginx/ssl/grafana.key \
  -out /etc/nginx/ssl/grafana.crt \
  -subj "/CN=Grafana"
```

Petit rappel : le certificat auto-signe n'est pas verifie par les navigateurs, vous aurez un ecran "Votre connexion n'est pas securisee". C'est normal. En production, on utilise Let's Encrypt (et c'est gratuit).
[/spoiler]

### Ajouter un bloc HTTPS
 - Ajoutez un second server block qui ecoute sur le port **443** avec SSL
 - Activez HTTP/2 (c'est plus rapide et quasi obligatoire de nos jours)
 - Limitez les protocoles TLS a `TLSv1.2` et `TLSv1.3` (pas de TLSv1.0 ou 1.1, ils sont obsoletes)
 - Ajoutez HSTS pour forcer le navigateur a utiliser HTTPS

[spoiler]
```nginx
server {
    listen 443 ssl http2;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/grafana.crt;
    ssl_certificate_key /etc/nginx/ssl/grafana.key;
    ssl_protocols       TLSv1.2 TLSv1.3;

    # Recommandations Mozilla pour les ciphers
    ssl_ciphers 'ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384';
    ssl_prefer_server_ciphers on;

    # HSTS
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # Le proxy vers Grafana (copie du bloc HTTP)
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
    }
}
```

```bash
nginx -t && systemctl reload nginx
```
[/spoiler]

### Rediriger HTTP vers HTTPS
Maintenant que vous avez le HTTPS, le bloc HTTP doit envoyer les visiteurs vers la version securisee.
 - Modifiez le bloc HTTP (port 80) pour faire une redirection 301 vers HTTPS
 - Testez que la redirection fonctionne

[spoiler]
```nginx
server {
    listen 80;
    server_name _;
    return 301 https://$host$request_uri;
}
```

Test :
```bash
curl -I http://[IP_VM]/
# Doit retourner 301 Location: https://.../
```
[/spoiler]

### Rate limiting
Un des avantages de NGinx, c'est qu'il peut limiter le nombre de requetes par seconde. Utile pour empecher les attaques par brute-force sur la page de login.
 - Ajoutez une zone de limit dans le bloc `http` de `nginx.conf`
 - Limitez la page `/login` a 5 requetes par seconde avec un burst de 5
 - Testez en envoyant 20 requetes rapides

[spoiler]
Dans `/etc/nginx/nginx.conf`, dans le bloc `http { }`, ajoutez :
```nginx
limit_req_zone $binary_remote_addr zone=login_limit:10m rate=5r/s;
```

Dans la config du site, ajoutez sur la location qui mene au login :
```nginx
location /login {
    limit_req zone=login_limit burst=5 delay=2;
    proxy_pass http://grafana_backend;
}
```

Test :
```bash
for i in $(seq 1 20); do
  curl -sk https://[IP_VM]/login -o /dev/null -w "%{http_code} "
done
```
Les premieres passent (200), puis vous devriez voir apparaitre des **503** quand le limit est depasse.
[/spoiler]

### Headers de securite
Ajoutez des headers HTTP pour renforcer la securite de votre site. Meme si ce n'est pas une protection absolue, c'est de la bonne pratique et ca fera plaisir a votre futur auditeur de securite.
 - `X-Content-Type-Options: nosniff`
 - `X-Frame-Options: SAMEORIGIN`
 - `X-XSS-Protection: 1; mode=block`
 - Cachez la version de NGinx avec `server_tokens off`

[spoiler]
Dans le bloc `server` de votre site HTTPS :
```nginx
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;

server_tokens off;
```

Test :
```bash
curl -skI https://[IP_VM]/ | grep -E "X-Content|X-Frame|Server"
# Le Server ne doit plus afficher la version !
```
[/spoiler]

## Final
Votre Grafana est maintenant accessible en HTTPS avec du rate limiting et des headers de securite. C'est deja beaucoup mieux, mais on peut encore ameliorer les performances.

Dans la suite, on va ajouter du cache, de la compression, et du monitoring pour savoir ce qui se passe sur notre serveur.

[05-Performances et Monitoring](./05-Performances%20et%20Monitoring.md)
