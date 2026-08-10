# Introduction
Ce TP a pour but de vous faire monter en competence sur NGinx, le serveur web le plus performant du marché, en le mettant en oeuvre dans un contexte professionnel : servir de **reverse proxy** pour **Grafana**, l'outil de monitoring et de dashboarding le plus populaire.

Ces compétences sont couramment demande dans le monde professionnel, dans le cadre de la mise en production d'application web. 

### Mise en situation
Vous venez d'etre embauche dans une TPE qui commence a avoir du traffic sur ses applications. Le lead dev, qui est aussi le patron, le commercial et le support, vous dit : "Nos vieux serveurs Apache commencent a ramer, on entend le vent dans les ventilos. Tu connais NGinx ? J'ai entendu dire que c'est plus leger et que ca tient mieux la charge. Tu nous fais ca pour la semaine prochaine ?". Ah et evidemment, vous n'avez pas le budget pour un adminsys. Vous allez devoir deployer **Grafana** (l'outil de monitoring de l'equipe) derriere un NGinx fraichement installe, avec le HTTPS, le cache, et tout le tralala.

#### Consignes & Philosophie
La mise en prod est decomposee en etapes logiques, mais c'est a vous de **determiner ce qu'il est nécessaire de faire**. Vous rencontrerez **des soucis techniques propres a vos choix** : résolvez-les ou posez-moi des questions, mais **documentez toujours**.

Ma vision, c'est qu'une personne compétente n'est pas quelqu'un **qui sait** mais quelqu'un **qui cherche a savoir**. Oui, en informatique, vos connaissances ont une date de péremption. Du genre courte, voire très courte.

Donc ne pas savoir c'est _normal et courant_, mais vous devez mettre un point d'honneur a trouver l'information. Et plus tôt vous vous y mettez et mieux vous saurez faire !

### Moyens
#### a l'IUT
Vous disposerez d'un serveur GNU/Linux sous la distribution Debian la plus recente et d'un acces via une interface web, qui est en fait une **VM sous ProxMox**.

Grace a mon incroyable mansuetude, vous aurez la possibilite de faire des "snapshots" de votre VM. **Servez-vous en**. Faites-en un de suite, d'ailleurs, vous risquez d'en avoir besoin plus vite que vous ne l'imaginez...

Pour rappel, on utilisera le meme type de VM que pour le TP Apache, mais cette fois **sans Apache installe**. NGinx va prendre la releve.

### Important
Vous **documenterez** tout. Mot de passe, doc sur internet, commande exotique, chemin de fichiers... Tout est potentiellement pertinent. A la fin du TP, vous devriez etre capable de confier ces notes a un tiers, sans le support de cours, et il doit pouvoir se debrouiller avec votre serveur.

[02-Prise en main du serveur](./02-Prise%20en%20main%20du%20serveur.md)
