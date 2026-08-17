---
title: "Déployer les sources"
parent: "TP Apache"
nav_order: 5
---
# Déployer les sources
Enfin, vous arrivez au bout de votre quête. Vos yeux saignent, vos doigts tremblent, et c'est avec émotions que vous aller mettre en place les sources du logiciel pour enfin le voir fonctionner.

<div class="astuce">Faites un snapshot !</div>

## Consignes
Let's fire this up ! 🚀🚀🚀
### Télécharger les sources
 - Trouver et téléchargez les sources sur votre serveur.

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Sincèrement, je suis désolé que vous ayez eu autant à souffrir. Le proxy de l'IUT c'est pas mon choix, mais c'est ainsi il faut faire avec, IRL ce sera plus facile. Aller configurez moi ce truc :</p>
<ul>
<li><code>export http_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'</code></li>
<li><code>export https_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'</code></li>
</ul>
<p>Et maintenant, téléchargez l'archive (je vous conseille de le faire dans votre <code>~</code> en tant que <strong>root</strong>):</p>
<ul>
<li><code>wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz</code></li>
</ul>
</div>
</details>

### Déployer les sources
 - Décompressez les et **rangez votre chambre**: on _veux_ un répertoire `/var/www/mantis/` qui contient les sources et pas un sous répertoire supplémentaire !
 - Mettre les bon droits ?! S'appuyer sur l'appendice : [droits et Apache](../Appendices/App.02 droits et Apache.md).

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>La partie facile :</p>
<ul>
<li><code>tar -xvf mantisbt-2.27.1.tar.gz</code></li>
<li><code>mv mantisbt-2.27.1 /var/www/mantis</code></li>
</ul>
<p>La partie plus touchy (en tant que <strong>root</strong> !). Pour faire simple, on va donner les droits <strong>750</strong> sur les répertoires, et <strong>640</strong> sur les fichiers. On va appliquer <code>root:www-data</code> en propriétaire. Allez dans <code>/var/www</code> :</p>
<ul>
<li><code>chown -R root:www-data mantis</code></li>
<li><code>chmod -R 750 mantis</code></li>
<li><code>find . -type f -print0 | xargs -0 chmod 640</code></li>
</ul>
<p>Comme on ne connait pas encore l'application, peut être que plus tard on aura à mettre des droits d'écriture sur certain répertoires. On passera alors <em>temporairement</em> ces répertoires à <strong>770</strong>, et nous nous féliciterons d'avoir conservé <code>www-data</code> en utilisateur.</p>
</div>
</details>

### Lancer l'installation

<div class="astuce">Faites un snapshot ! Oui maintenant, avant de cliquer quoi que ce soit ! (<i>bon sang mon grand cœur me perdra</i>)</div>

 
 - Lancer l'installation de **Mantis** en complétant les paramètres avec vos notes.


<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Remplissez les paramètres d'installation... il vous demande :</p>
<ul>
<li><em>Username (for Database)</em> : inventez un nouvel utilisateur qui servira à se connecter à la BDD (et <strong>pas</strong> root !)</li>
<li><em>Password (for Database)</em> : donner lui un mot de passe</li>
<li><em>Admin Username (to create Database if required)</em> : <strong>root</strong>, que vous avez configuré en installant <em>mariadb</em></li>
<li><em>Admin Password (to create Database if required)</em> : ...et le mot de passe que vous avez noté</li>
</ul>
<p>Et lorsque l'on clique sur continuer, il y a quelques soucis ! Il nous indique plusieurs erreurs. <strong>Pas de panique</strong>. Restez bien sur la page.</p>
<ul>
<li>_cannot write /var/www/mantis/config/config_inc.php_ : Aïe ! Donnons lui des droits large sur le répertoire en question, nous corrigerons par la suite : <code>chmod -R 770 mantis/config</code></li>
<li>_Database user doesn't have access to the database ( Access denied for user 'mantis_user'@'localhost' )_ : Bon sang, il n'a pas été capable de se donner les bons droits sur sa propre BDD ! Connectons nous sur la BDD : <code>mariadb -u root -p</code> et changeons les droits sur la base de donnée visible : <code>GRANT ALL PRIVILEGES ON bugtracker.* TO 'mantis_user'@'localhost' IDENTIFIED BY '[VOTRE_MOT_DE_PASSE_POUR_CET_USER]';</code> (attention à la fatigue, mettez bien le <strong>bon</strong> user, le <strong>bon</strong> mot de passe !)</li>
</ul>
<p>Maintenant, faites "<em>précédent</em>" sur votre navigateur, puis "<em>Install/Upgrade Database</em>".... Et il devrait vous indiquer en bas "<em>MantisBT was installed successfully. Continue to log in.</em>"</p>
<p>Enfin, cliquez sur <em>Continuez</em> et authentifiez vous, comme dans la doc, avec comme login <code>administrator</code> et mot de passe <code>root</code>.</p>
<p><strong>Et si ça ne marche pas pour moi?</strong>.</p>
<ul>
<li>Reprenez le snapshot précédent où tout allait bien</li>
<li>Aidez vous des logs d'apache : <code>tail -f /var/log/apache2/error.log</code></li>
</ul>
</div>
</details>

## Final
Bon sang ! Déjà fini ? Mais comment diantre ?! Super, vous avez le temps de **restaurer votre snapshot et recommencer** !

Après ça, il ne nous reste plus que la partie "consolidation", où l'on va tenter de renforcer notre machine vis-à-vis du monde hostile qui nous entoure.



