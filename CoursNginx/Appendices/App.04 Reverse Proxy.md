---
title: "Appendice 4 : Reverse proxy, mode d'emploi"
parent: "Appendices Nginx"
nav_order: 24
---
# Appendice 4 : Reverse proxy, mode d'emploi

Un reverse proxy, c'est un serveur qui se place **devant** un ou plusieurs serveurs backend et qui relaie les requetes.

### Schema
```
Client -> NGinx (80/443) -> Backend (localhost:3000)
               |
               +-> Cache
               +-> TLS
               +-> Rate limiting
               +-> Load balancing
```

### Pourquoi un reverse proxy ?
- **Securite** : le backend est invisible depuis l'exterieur
- **TLS** : un seul point de terminaison HTTPS
- **Cache** : soulage le backend
- **Agregation** : plusieurs services sur un meme domaine (Grafana + Prometheus + API)
- **Load balancing** : repartir la charge entre plusieurs instances

### Headers importants
Quand NGinx relaie une requete, le backend ne voit pas l'IP du client mais celle de NGinx. Il faut transmettre les bonnes infos :

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

### Upstream et load balancing
```nginx
upstream mon_app {
    least_conn;
    server 127.0.0.1:3000 weight=3;
    server 127.0.0.1:3001 weight=1 backup;
}
```

Algorithmes : `least_conn` (moins de connexions), `ip_hash` (sticky session), `random`, ou par defaut round-robin.

Retour aux [chapitres](../Sommaire Nginx.md).
