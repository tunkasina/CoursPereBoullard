---
title: "Appendice 2 : Droits et NGinx"
parent: "Appendices"
nav_order: 22
---
# Appendice 2 : Droits et NGinx

NGinx tourne sous l'utilisateur `www-data` (comme Apache). Les fichiers servis par NGinx doivent etre lisibles par cet utilisateur.

### Regle de base
```bash
chown -R root:www-data /var/www/grafana-project
chmod -R 750 /var/www/grafana-project
```

Les dossiers en **750** (rwxr-x---), les fichiers en **640** (rw-r-----):

```bash
find /var/www/grafana-project -type d -exec chmod 750 {} \;
find /var/www/grafana-project -type f -exec chmod 640 {} \;
```

### Pourquoi pas 755/644 ?
En **750**, les autres utilisateurs du systeme (qui ne sont pas `www-data`) ne peuvent pas lire vos fichiers. C'est un peu plus restrictif, mais plus secure. On est pas la pour faire de l'hebergement mutualiste avec 50 clients.

Retour aux [chapitres](../Sommaire Nginx.md).
