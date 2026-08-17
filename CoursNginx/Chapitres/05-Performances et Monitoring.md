---
title: "Performances et Monitoring"
parent: "TP Nginx"
nav_order: 5.5
---
# Performances et Monitoring
Votre serveur est fonctionnel et sécurisé. Voyons comment optimiser les performances (compression, cache) et mettre en place du monitoring basique.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires...

### Activer la compression
 - Activez `gzip` dans la configuration générale de Nginx pour compresser les fichiers textes (CSS, JS, JSON).

<details class="spoiler"><summary>Solution / Indice</summary>
Dans `/etc/nginx/nginx.conf` (`http { }`) :
```nginx
gzip on;
gzip_types text/plain text/css application/json application/javascript image/svg+xml;
```
Vérification : `curl -H "Accept-Encoding: gzip" -I http://[IP_VM]/`
</details>

### Configurer `stub_status`
 - Activez le module de statistiques de Nginx pour surveiller l'état des connexions en temps réel.

<details class="spoiler"><summary>Solution / Indice</summary>
Dans un bloc `server` interne (ex: sur le port 8080 local) :
```nginx
server {
    listen 127.0.0.1:8080;
    location /nginx_status {
        stub_status;
        allow 127.0.0.1;
        deny all;
    }
}
```
Test : `curl http://127.0.0.1:8080/nginx_status`
</details>

## Final
Votre cours Nginx est complet, aligné avec la philosophie du cours Apache et prêt pour vos étudiants !

[Retour au sommaire](../Sommaire%20Nginx.md)
