# Déployer les sources
C'est le moment tant attendu : mettre en place les sources de **MantisBT** pour enfin voir le résultat.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Télécharger les sources
 - Téléchargez l'archive de MantisBT sur votre serveur.

[spoiler]
Utilisez `wget` :
`wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz`
[/spoiler]

### Déployer les sources
 - Décompressez l'archive dans `/var/www/mantis/`.
 - Configurez les droits d'accès en vous appuyant sur [l'appendice sur les droits](CoursNginx/Appendices/App.02%20droits%20et%20Nginx.md).

[spoiler]
 - `tar -xvf mantisbt-2.27.1.tar.gz`
 - `mv mantisbt-2.27.1 /var/www/mantis`
 - `chown -R root:www-data /var/www/mantis`
 - `chmod -R 750 /var/www/mantis`
 - `find /var/www/mantis -type f -exec chmod 640 {} +`
[/spoiler]

### Lancer l'installation
Lancer l'installation via votre navigateur sur `https://[VOTRE_IP]`.

[spoiler]
Lors de l'installation :
 - **Database User** : Créez un utilisateur dédié (ex: `mantis_user`).
 - **Admin Password** : Utilisez le mot de passe root de MariaDB.

**Erreurs classiques :**
- `cannot write config_inc.php` : Donnez temporairement des droits d'écriture : `chmod -R 770 /var/www/mantis/config`.
- `Access denied for user 'mantis_user'@'localhost'` : Donnez les droits sur la BDD :
  `mariadb -u root -p`
  `GRANT ALL PRIVILEGES ON bugtracker.* TO 'mantis_user'@'localhost' IDENTIFIED BY 'MotDePasse!';`

Une fois terminé, repassez les droits du dossier `config` en **750**.
[/spoiler]

## Final
Félicitations, votre bug tracker est en ligne ! Mais est-il sécurisé ? On voit ça dans la partie consolidation.

[06-Consolidation](./06-Consolidation.md)
