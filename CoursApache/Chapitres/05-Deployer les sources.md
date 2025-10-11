### Déployer les sources
Enfin, vous arrivez au bout de votre quête. Vos yeux saignent, vos doigts tremblent, et c'est avec émotions que vous aller mettre en place les sources du logiciel pour enfin le voir fonctionner.

<div class="astuce">Faites un snapshot !</div>

## Consignes
🚀🚀🚀

### Télécharger les sources
 - Trouver et téléchargez les sources sur votre serveur.

[spoiler]
Sincèrement, je suis désolé que vous ayez eu autant à souffrir. Le proxy de l'IUT c'est pas mon choix, mais c'est ainsi il faut faire avec, IRL ce sera plus facile. Aller configurer moi ce truc :
 - `export http_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`
 - `export https_proxy='http://proxy.iutbourg.univ-lyon1.fr:3128'`

Et maintenant, téléchargez l'archive (je vous conseille de le faire dans votre `~` en tant que **root**):
 - `wget https://netcologne.dl.sourceforge.net/project/mantisbt/mantis-stable/2.27.1/mantisbt-2.27.1.tar.gz`

[/spoiler]

### Déployer les sources
 - Décompressez les et **rangez votre chambre**: on _veux_ un répertoire `/var/www/mantis/` qui contient les sources et pas un sous répertoire supplémentaire !
 - Mettre les bon droits ?! S'appuyer sur l'appendice : [droits et Apache](https://tunkasina.github.io/CoursPereBoullard/#/./CoursApache/Chapitres/App.02%20droits%20et%20Apache.md).

[spoiler]
La partie facile :
 - `tar -xvf mantisbt-2.27.1.tar.gz`
 - `mv mantisbt-2.27.1 /var/www/mantis`

La partie plus touchy (en tant que **root** !):
- qsd
- qsdqs
- sqd
- sq
- sqd
- qsd
- 
`
[/spoiler]

## Final
Bon sang ! Déjà fini ? Mais comment diantre ?! Super, vous avez le temps de **restaurer votre snapshot et recommencer** !
