---
title: "Déployer les sources"
parent: "TP Nginx"
nav_order: 5
---
# Déployer les sources
Vous arrivez à la dernière étape avant de voir fonctionner votre bug tracker Mantis sous Nginx.

<div class="astuce">Faites un snapshot !</div>

## Consignes
### Télécharger et décompresser Mantis
 - Téléchargez l'archive de MantisBT (version 2.27.1 par exemple, comme pour la voie Apache).
 - Décompressez-la et rangez les sources dans `/var/www/mantis`.
 - Appliquez les bons droits et propriétaires (`root:www-data`, permissions `750` / `640`).

<details class="spoiler">
<summary>Solution</summary>
<div class="spoiler-content">
<p>```bash</p>
<p>wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz</p>
<p>tar -xvf mantisbt-2.27.1.tar.gz</p>
<p>mv mantisbt-2.27.1 /var/www/mantis</p>
<p>chown -R root:www-data /var/www/mantis</p>
<p>find /var/www/mantis -type d -exec chmod 750 {} \;</p>
<p>find /var/www/mantis -type f -exec chmod 640 {} \;</p>
<p>```</p>
</div>
</details>

### Lancer l'installation dans le navigateur
 - Rendez-vous sur `https://[IP_VM]/` pour lancer l'installateur web de Mantis.
 - Remplissez les informations de base de données (créez un utilisateur dédié `mantis_user`, connectez-vous avec `root` de MariaDB pour initialiser la base).
 - Résolvez les éventuels problèmes de droits d'écriture sur le dossier `config` (`chmod -R 770 config`).

<details class="spoiler">
<summary>Solution</summary>
<div class="spoiler-content">
<p>Même logique que sous Apache : l'installateur crée la base et les tables. Si la base refuse l'accès, accordez les privilèges via le CLI MariaDB :</p>
<p>```sql</p>
<p>GRANT ALL PRIVILEGES ON bugtracker.* TO 'mantis_user'@'localhost' IDENTIFIED BY '[VOTRE_MDP]';</p>
<p>```</p>
</div>
</details>

## Final
Mantis est déployé et opérationnel sous **Nginx + PHP-FPM + MariaDB** ! 

[06-Consolidation](./06-Consolidation.md)
