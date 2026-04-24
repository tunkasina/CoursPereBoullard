# Introduction
Ce TP a pour but de vous faire découvrir la prise en main du système le plus populaire dans l’hébergement Web, GNU/Linux, dans sa distribution la plus courante, Debian. Vous déploierez des services omniprésents sur Internet (Nginx, MariaDB, SSH…).

Ces compétences sont couramment demandées dans le monde professionnel, dans le cadre de la mise en production d’application web. Elles vous permettront, soit de faire la mise en œuvre par vous-même, soit d’avoir une bonne communication avec l’administrateur système de votre organisation.

### Pourquoi Nginx ?
Alors que nous avons vu Apache, **Nginx** (prononcez "Engine-X") est devenu incontournable pour ses performances, sa légèreté et sa capacité à gérer un grand nombre de connexions simultanées. Il est souvent utilisé comme serveur web, mais aussi comme reverse-proxy ou répartiteur de charge (load balancer).

### Mise en situation
Vous venez d’être embauché dans une TPE, et le lead dev vous demande de mettre en place un bug tracker. On ne va pas embaucher un adminsys pour ça quand même, nan mais t'es informaticien ou pas ? Vous devrez donc réaliser la mise en production de **"Mantis Bug Tracker"** en utilisant cette fois **Nginx**.  
  
#### Consignes & Philosophie
La mise en prod' est découpée en étapes logiques, mais c’est à vous de **déterminer ce qu’il est nécessaire de faire**. Vous rencontrerez **des soucis techniques propres à vos choix** : résolvez-les ou posez-moi des questions, mais **documentez toujours**.

Ma vision, c'est qu'une personne compétente n'est pas quelqu'un **qui sait** mais quelqu'un **qui cherche à savoir**. Oui, en informatique, vos connaissances ont une date de péremption. Du genre courte, voire très courte.

### Moyens
#### à l'IUT
Vous disposerez d’un serveur GNU/Linux sous Debian le plus récent via une interface web (VM sous ProxMox).  

Grâce à mon incroyable mansuétude, vous aurez la possibilité de faire des "snapshots" de votre VM. **Servez-vous en**. Faites-en un de suite, d'ailleurs !

![LinuxNginx-00-01.png](../Images/LinuxNginx-00-01.png)

#### à la maison
Si vous voulez reproduire ce TP à la maison, il faudra passer par une VM (VirtualBox, VMWare, QEMU KVM...).

### Important
Vous **documenterez** tout. Mot de passe, doc sur internet, commande exotique, chemin de fichiers… Tout est potentiellement pertinent. A la fin du TP, vous devriez être capable de confier ces notes à un tiers, sans le support de cours, et il doit pouvoir se débrouiller avec votre serveur.

[02-Prise en main du serveur](./02-Prise%20en%20main%20du%20serveur.md)
