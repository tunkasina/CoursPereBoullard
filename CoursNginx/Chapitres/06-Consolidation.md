# Consolidation
La **Consolidation** consiste à durcir le serveur pour limiter la surface d'attaque.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Passif : Limiter l'exposition
#### Post-installation
 - Appliquez les recommandations de sécurité de Mantis (changer le mot de passe admin, supprimer le dossier `/admin`).
 - Verrouillez les droits du dossier `/var/www/mantis/config` (640/750).

#### Discrétion
 - Obtenez un 404 et analysez les headers.
 - Masquez la signature de **Nginx**.

[spoiler]
Vérifiez avec `curl -I http://[VOTRE_IP]`. Vous verrez `Server: nginx/1.x.x (Debian)`.
Pour masquer cela, éditez `/etc/nginx/nginx.conf` et ajoutez dans la section `http { ... }` :
`server_tokens off;`
Puis : `systemctl reload nginx`.
[/spoiler]

#### Seulement l'utile
 - Vérifiez vos ports ouverts : `ss -tul`.

[spoiler]
Vous devriez voir :
 - Port 22 (SSH) $\rightarrow$ 0.0.0.0
 - Port 80/443 (Nginx) $\rightarrow$ 0.0.0.0
 - Port 3306 (MariaDB) $\rightarrow$ 127.0.0.1 (C'est correct, la BDD ne doit pas être exposée !)
[/spoiler]

### Actif : Maintenance et résilience
 - Mettez en place un script de sauvegarde de la base de données via `cron`.
 - Ajoutez un rappel de mise à jour dans le `.bashrc`.

[spoiler]
Script `backup_mariadb.sh` :
`#!/bin/bash`
`DATE=$(date +%F_%H-%M)`
`mysqldump -u mantis_user -p'MotDePasse!' bugtracker > /var/backups/mantisbt_$DATE.sql`
Puis `crontab -e` $\rightarrow$ `0 2 * * * /root/backup_mariadb.sh`
[/spoiler]

### Proactif : Protection
 - Installer et configurer **fail2ban** pour protéger SSH.

[spoiler]
`apt install fail2ban`
Éditez `/etc/fail2ban/jail.conf` pour ajouter votre IP dans `ignoreip`.
Redémarrez : `systemctl restart fail2ban`.
[/spoiler]

## Final
C'est la fin du cours Nginx ! Vous avez maintenant une vision comparative entre Apache et Nginx, et vous savez déployer une application web de A à Z sur Debian.

**Bravo pour vos efforts !**
