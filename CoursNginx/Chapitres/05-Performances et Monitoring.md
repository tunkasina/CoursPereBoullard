# Performances et Monitoring
Votre serveur est fonctionnel et securise, mais tient-il vraiment la charge ? NGinx est repute pour ses performances, encore faut-il les configurer correctement. On va aussi mettre en place du monitoring pour savoir ce qu'il se passe.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous meme et par tous les moyens necessaires... Vous commencez a avoir l'habitude, non ?

### Activer la compression
Les fichiers CSS, JS, JSON se compressent tres bien. NGinx peut les compresser a la volee avant de les envoyer au client. Ca reduit la bande passante et ca accelere le chargement.
 - Activez gzip dans la configuration NGinx
 - Compressez les types courants (CSS, JS, JSON, SVG)
 - Testez que la compression fonctionne

[spoiler]
Dans `/etc/nginx/nginx.conf`, dans le bloc `http { }` :
```nginx
gzip on;
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_types text/plain text/css application/json
           application/javascript text/xml application/xml
           image/svg+xml;
gzip_min_length 256;
```

Test :
```bash
curl -H "Accept-Encoding: gzip" -I http://[IP_VM]/static/css/style.css
# Vous devriez voir "Content-Encoding: gzip" dans la reponse
```

Si vous ne voyez pas le header, verifiez que le fichier fait plus de 256 octets (merci `gzip_min_length`).
[/spoiler]

### Configurer le cache proxy
Actuellement, chaque requete a Grafana est transmise au backend. NGinx peut mettre en cache les reponses pour eviter de solliciter Grafana a chaque fois.
 - Creez un repertoire de cache
 - Configurez `proxy_cache_path` dans `nginx.conf`
 - Activez le cache sur la location `/static/` (ou tout le site)
 - Ajoutez un header `X-Cache-Status` pour voir si la reponse vient du cache
 - Testez : la premiere requete doit etre MISS, la seconde HIT

[spoiler]
Dans `/etc/nginx/nginx.conf`, dans le bloc `http { }` :
```nginx
proxy_cache_path /var/cache/nginx levels=1:2
                 keys_zone=grafana_cache:10m max_size=1g
                 inactive=60m use_temp_path=off;
```

Dans la config du site, sur la location ou vous voulez du cache :
```nginx
location / {
    proxy_cache grafana_cache;
    proxy_cache_key "$scheme$request_method$host$request_uri";
    proxy_cache_valid 200 302 10m;
    proxy_cache_valid 404 1m;
    proxy_cache_use_stale error timeout updating http_500;
    proxy_pass http://grafana_backend;
    add_header X-Cache-Status $upstream_cache_status;
}
```

Test :
```bash
curl -skI https://[IP_VM]/ | grep X-Cache-Status
# MISS
curl -skI https://[IP_VM]/ | grep X-Cache-Status
# HIT
```
[/spoiler]

### Optimiser les performances generales
Quelques reglages simples peuvent faire une grosse difference sur des serveurs charges.
 - Ajustez `worker_processes` a `auto`
 - Augmentez `worker_connections`
 - Activez `sendfile`, `tcp_nopush`, `tcp_nodelay`
 - Ajustez les timeouts

[spoiler]
Dans `/etc/nginx/nginx.conf` :
```nginx
worker_processes auto;
worker_rlimit_nofile 65535;

events {
    worker_connections 4096;
    use epoll;
    multi_accept on;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    keepalive_requests 100;
}
```

`sendfile` permet a NGinx d'envoyer les fichiers sans passer par un buffer user-space (zero-copy). `tcp_nopush` optimise l'envoi des paquets. `tcp_nodelay` desactive l'algorithme de Nagle pour les connexions interactives.
[/spoiler]

### Mettre en place stub_status
NGinx a un module tout bete qui donne des metriques en temps reel : `stub_status`. Ca permet de savoir combien de connexions sont actives, combien de requetes ont ete traitees, etc.
 - Ajoutez un server block dedie ecoutant sur `127.0.0.1:8080`
 - Activez `stub_status` sur une location `/nginx_status`
 - Restreignez l'acces a localhost uniquement
 - Testez avec curl

[spoiler]
Dans une nouvelle config ou dans le fichier principal :
```nginx
server {
    listen 127.0.0.1:8080;
    access_log off;

    location /nginx_status {
        stub_status;
        allow 127.0.0.1;
        deny all;
    }
}
```

Test :
```bash
curl http://127.0.0.1:8080/nginx_status
```
Reponse typique :
```
Active connections: 42
server accepts handled requests
 12345 12345 67890
Reading: 0 Writing: 3 Waiting: 39
```
- **Active connections** : connexions en cours
- **accepts** : connexions acceptees depuis le demarrage
- **handled** : connexions traitees (devrait = accepts)
- **Reading** : headers en cours de lecture
- **Writing** : reponse en cours d'envoi
- **Waiting** : keepalive en attente

Vous pouvez brancher Prometheus la-dessus avec l'exporteur officiel, puis monitorer NGinx depuis... Grafana ! La boucle est bouclee.
[/spoiler]

### Benchmark avant/apres
Pour voir si toutes ces optimisations servent a quelque chose, faisons un petit benchmark.
 - Installez `apache2-utils` (oui, il contient `ab`, le benchmark HTTP)
 - Testez les performances sur un fichier statique sans cache
 - Activez le cache et re-testez
 - Comparez les resultats

[spoiler]
```bash
apt install -y apache2-utils

# Sans cache (si vous avez un fichier statique)
ab -n 500 -c 10 http://[IP_VM]/static/css/style.css

# Avec cache
ab -n 500 -c 10 http://[IP_VM]/static/css/style.css
```

Comparez les lignes "Requests per second" et "Time per request". Normalement le cache fait une difference notable. Si ce n'est pas le cas, c'est que votre fichier est tout petit et que le bottleneck n'est pas la. Essayez avec une image plus grosse.
[/spoiler]

## Final
Votre infrastructure est maintenant operationnelle, securisee et optimisee. Vous avez :
 - Un reverse proxy NGinx devant Grafana
 - Du HTTPS avec des headers de securite
 - Du rate limiting pour eviter les abus
 - Du cache et de la compression
 - Un point de monitoring stub_status

Vous pouvez etre fiers de vous. A partir de la, les possibilites sont infinies : ajouter Prometheus, load-balancer sur plusieurs instances Grafana, automatiser le deploiement avec Ansible...

Mais tout ca, c'est pour plus tard. @Bientot !

[Retour au sommaire](../Sommaire%20Nginx.md)
