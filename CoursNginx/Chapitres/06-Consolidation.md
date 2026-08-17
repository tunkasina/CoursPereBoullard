---
title: "Consolidation"
parent: "TP Nginx"
nav_order: 6
---
# Consolidation
Comme pour la voie Apache, nous allons renforcer notre serveur Nginx face aux attaques.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Discrétion et en-têtes
 - Masquez la version de Nginx dans les réponses HTTP.

<details class="spoiler"><summary>Solution / Indice</summary>
Dans `/etc/nginx/nginx.conf`, dans le bloc `http { }` :
```nginx
server_tokens off;
```
Puis testez avec `curl -skI https://[IP_VM]/`. Le header `Server` ne doit plus afficher la version exacte de Nginx.
</details>

### Sécurité active et proactif
 - Mettez en place la sauvegarde automatique de votre base de données MariaDB via un script et une tâche `cron`.
 - Installez et configurez `fail2ban` pour protéger l'accès SSH.

<details class="spoiler"><summary>Solution / Indice</summary>
Identique au parcours Apache :
 - Script `backup_mariadb.sh` avec `mysqldump`
 - `apt install fail2ban` et configuration de la jail `sshd`.
</details>

## Final
Félicitations ! Vous avez mené à bien la mise en production de Mantis sous **Nginx**. Vous maîtrisez à la fois l'approche Apache et l'approche Nginx / PHP-FPM pour 30 étudiants sereins et un support unifié !

[Retour au sommaire](../Sommaire%20Nginx.md)
