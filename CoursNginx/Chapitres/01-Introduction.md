# Introduction
Ce TP a pour but de vous faire découvrir la mise en production d'une application web sous **GNU/Linux Debian**, en utilisant **Nginx** comme serveur web et reverse proxy.

### Mise en situation
Vous venez d'être embauché dans une TPE, et le lead dev (qui est aussi le patron, l'équipe marketing et la RH) vous demande de mettre en place un bug tracker : **"Mantis Bug Tracker"**. Vos collègues en salle machine utilisent Apache, mais vous avez carte blanche pour tester Nginx. Vous devrez donc réaliser la mise en production de Mantis derrière Nginx.

#### Consignes & Philosophie
La mise en prod' est découpée en étapes logiques, mais c’est à vous de **déterminer ce qu’il est nécessaire de faire**. Vous rencontrerez **des soucis techniques propres à vos choix** : résolvez-les ou posez-moi des questions, mais **documentez toujours**.

Ma vision, c'est qu'une personne compétente n'est pas quelqu'un **qui sait** mais quelqu'un **qui cherche à savoir**. Vos connaissances ont une date de péremption courte. Ne pas savoir est normal, mais vous devez mettre un point d'honneur à trouver l'information.

### Moyens
#### À l'IUT
Vous disposerez d'un serveur GNU/Linux sous Debian et d'un accès via une VM sous **Proxmox**.
Grâce à mon incroyable mansuétude, vous aurez la possibilité de faire des **snapshots** de votre VM. **Servez-vous en**.

#### À la maison
Passez par une VM (VirtualBox, VMware, QEMU/KVM...).

### Important
Vous **documenterez** tout. Mots de passe, doc sur Internet, commandes, chemins de fichiers... À la fin du TP, vos notes doivent permettre à un tiers de se débrouiller avec votre serveur.

[02 - Prise en main du serveur](./02-Prise%20en%20main%20du%20serveur.md)
