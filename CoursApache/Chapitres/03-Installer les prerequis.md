---
title: "Installer les prérequis"
parent: "TP Apache"
nav_order: 3
---
# Installer les prérequis
L'application Web que nous déployons a besoin de prérequis sur le serveur. C'est le cas de toutes les applications, c'est également le cas de celles que vous coderez.  (Bien sûr vous pourriez aussi trouvez des containers à déployer, mais ces containers, ils sont réalisés comment au départ ? … )  Donc, pour **Mantis**, il faut lire sa doc et trouver ses prérequis.  

(_Spoiler Alert?_) Ce sera forcément Apache, MariaDB, PHP…  Mais quelles versions ? Quelles modules ? Quels sont les modules obligatoires, ou non ?

<div class="astuce">Faites un snapshot !</div>

## Consignes
Evidemment, vous chercherez par vous même et par tout les moyens nécessaires, les commandes à taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne mémorisera rien dans ce cas.
### Trouver les prérequis
 - Compatibilité avec Debian ?
 - Les logiciels et leurs versions
 - les modules et leurs versions
Un bonne recherche sur Internet devrait vous aider !

<details class="spoiler">
<summary>Solution</summary>
<div class="spoiler-content">
<p>Cherchez sur le web <code>insite:mantisbt.org mantis server requisite</code>. </p>
<p>Et sur la page des prérequis logiciel, cherchez la <code>Versions compatibility table</code></p>
<p>Cela nous permet de savoir que l'on doit installer :</p>
<ul>
<li> <em>apache2</em> (>=2.4.13) comme serveur web</li>
<li><em>mariadb</em> (>=5.5.35) comme SGBD</li>
<li><em>php</em> (>=7.4) comme interpréteur de script</li>
<li>les modules php <em>mbstring</em> et <em>mysql</em></li>
</ul>
<p>De plus il est clairement dit dans la documentation que l'OS importe peu tant qu'il peut faire tourner les prérequis listés !</p>
<p>Quoi c'est trop vieux jeu pour vous une recherche web ? </p>
<p>Bon bah demandez à Gemini ou ChatGPT ou Mistra : </p>
<p>"<em>Je veux installer Mantis sur Apache, donne moi les versions exacte des logiciel et modules PHP nécessaire <strong>en me donnant la source, et les liens vers la documentation officielle</strong></em>"</p>
<p>En formulant ainsi, vous le forcez à vous donner les informations qu'il a utilisé pour vous régurgiter l'information, et pouvez vérifier que c'est à jour... ou pas !</p>
</div>
</details>

### Installer les prérequis
 - Installer les prérequis que vous avez précédemment trouvés

<details class="spoiler">
<summary>Solution</summary>
<div class="spoiler-content">
<ul>
<li>Pour installer apache : <code>apt install apache2</code></li>
<li>Pour installer mariadb : <code>apt install mariadb-server</code></li>
<li>Pour installer PHP et ses modules : <code>apt install php php-mbstring php-mysql</code></li>
</ul>
<p>(<em>oui on est d'accord, pas de quoi se taper la tête contre les murs</em>)</p>
</div>
</details>

### Vérifier les prérequis
 - Vérifier que _apache_ fonctionne
 - Vérifier que _mariadb_ fonctionne
 - Vérifier que _PHP_ fonctionne

<details class="spoiler">
<summary>Solution</summary>
<div class="spoiler-content">
<ul>
<li>Dans un premier temps, on vérifie que les différents démons sont fonctionnels : <code>systemctl status apache2.service</code>, <code>systemctl status mariadb.service</code> - qui affiche le statut et un extrait des logs.</li>
<li>Ensuite, une petite visite sur <code>http://[ip de votre serveur]</code> vous permet de voir que <em>apache</em> vous rend bien une page web</li>
<li>Pour vérifier que _PHP_ fonctionne, il faut simplement se créer une page **/var/www/html/index.php** avec comme contenu `<?php  phpinfo();`. Y accéder via `http://[ip de votre serveur]/index.php` devrait vous renvoyer une bien belle page d'info sur votre interpréteur PHP et ses modules.</li>
</ul>
</div>
</details>

## Final
Et maintenant ... ? **STOP** ! On configurera à la prochaine étape !

Comme la dernière fois, si avec vos notes ne vous permettent pas de refaire cette partie... **restaurez votre snapshot et recommencez** ! (_bon j'avoue là normalement, c'est peanuts_)

