---
title: "Installer les prérequis"
parent: "TP Nginx"
nav_order: 3
---
# Installer les prérequis
L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce soir c'est un serveur web (Nginx), MariaDB, PHP... Mais quelles versions ? Quels modules ?

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tous les moyens nécessaires, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.
### Trouver les prérequis
 - Compatibilité avec Debian ?
 - Les logiciels et leurs versions
 - Les modules PHP nécessaires
Un bonne recherche sur Internet devrait vous aider !

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<p>Cherchez sur le web <code>insite:mantisbt.org mantis server requisite</code>. </p>
<p>Et sur la page des prérequis logiciel, cherchez la <code>Versions compatibility table</code></p>
<p>Cela nous permet de savoir que l'on doit installer :</p>
<ul>
<li><em>nginx</em> (>=1.10.x)comme serveur web</li>
<li><em>mariadb</em> (>=5.5.35) comme SGBD</li>
<li><em>php</em> (>=7.4) comme interpréteur de script</li>
<li>les modules php <em>mbstring</em> et <em>mysql</em></li>
<li><em>php-fpm</em> (car Nginx, contrairement à Apache, ne gère pas PHP en interne et a besoin d'un gestionnaire de processus externe).</li>
</ul>
<p>De plus il est clairement dit dans la documentation que l'OS importe peu tant qu'il peut faire tourner les prérequis listés !</p>
<p>Quoi c'est trop vieux jeu pour vous une recherche web ? </p>
<p>Bon bah demandez à Gemini ou ChatGPT ou Mistra : </p>
<p>"<em>Je veux installer Mantis sur NGinx, donne moi les versions exacte des logiciel et modules PHP nécessaire <strong>en me donnant la source, et les liens vers la documentation officielle</strong></em>"</p>
<p>En formulant ainsi, vous le forcez à vous donner les informations qu'il a utilisé pour vous régurgiter l'information, et pouvez vérifier que c'est à jour... ou pas !</p>
</div>
</details>

### Installer les prérequis
 - Installez les prérequis que vous avez précédemment trouvés.

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<ul>
<li>Pour installer Nginx : <code>apt install nginx</code></li>
<li>Pour installer MariaDB : <code>apt install mariadb-server</code></li>
<li>Pour installer PHP et ses modules : <code>apt install php php-mbstring php-mysql php-fpm</code></li>
</ul>
</div>
</details>

### Vérifier les prérequis
 - Vérifiez que Nginx fonctionne.
 - Vérifiez que MariaDB fonctionne.
 - Vérifiez que PHP et PHP-FPM fonctionnent.

<details class="spoiler">
<summary>Solution / Indice</summary>
<div class="spoiler-content">
<ul>
<li>Dans un premier temps, on vérifie que les démons sont fonctionnels : <code>systemctl status nginx.service</code>, <code>systemctl status mariadb.service</code>, <code>systemctl status php-fpm</code> (ou version correspondante).</li>
<li>Ensuite, pour tester l'interprétation PHP avec Nginx, créez une page `/var/www/html/index.php` avec `<?php phpinfo(); ?>` (attention, sous Nginx il vous faudra configurer le bloc `location ~ \.php$` pointant vers le socket PHP-FPM pour l'afficher correctement).</li>
</ul>
</div>
</details>

## Final
Et maintenant ... ? **STOP** ! On configurera à [04-Configurer les prerequis](../../CoursApache/Chapitres/04-Configurer les prerequis.md) !

Comme la dernière fois, si vos notes ne vous permettent pas de refaire cette partie... **restaurez votre snapshot et recommencez** !
