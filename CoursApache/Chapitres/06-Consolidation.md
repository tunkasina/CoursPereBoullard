# Consolidation
On appelle **Consolidation** le fait de renforcer notre serveur face aux attaques potentielles.Bien entendu, vous ne savez pas comment s'organise une attaque et heureusement, j'ai pensé à vous.

Allez sur [une attaque informatique](https://tunkasina.github.io/CoursPereBoullard/#/./Appendices/App.04%20attaque%20informatique.md) pour en savoir plus, avant de faire la suite !

<div class="astuce">Faites un snapshot !</div>

## Consignes
Vous n'aurez pas le temps de tout faire. Vous devrez choisir parmi les rubriques suivantes, ce qui vous intéresse plus ... Vous ne serez pas évalué dessus, bien sûr !

### Passif
Cela consiste à limiter ce que l'on offre de manière passive, aux potentiels agresseurs. Voyez cela comme le fait de ne pas laisser la clé sur la serrure, mais aussi comme le fait d'éviter de mettre une grosse flèche rouge vers votre coffre-fort, ou de laisser la porte du jardin ouverte.
 - Appliquez les instructions post-installation de **Mantis** 
 - Contrôler les droits utiles et nécessaire sur `/var/www/mantis`
 - Diminuez la signature d'**Apache** et de **PHP**
 - Vérifier vos port ouverts vers l’extérieur

[spoiler]
A rédiger
[/spoiler]

### Actif
Cela consiste à mener des actions de protection, afin d'augmenter la résilience de notre système. C'est la maintenance de notre système, et des bonnes pratiques, de bonne santé.
 - Mettre en place un script de sauvegarde de la base de donnée
 - Mettre en place un script de vérification de mise à jours dans le `.bashrc` (qui s'affichera donc dès que vous vous connectez sur le serveur)

[spoiler]
A rédiger
[/spoiler]

### Proactif
Ce sont des réactions à mettre en oeuvre face à certains événements. Typiquement, ce sera votre système d'alarme.
 - mettre en place un `fail2ban`, et le configurer pour laisser passer une ip de votre choix, et bannir toutes les autres IP qui échouerai leur connexion **SSH** trois fois
 - en se basant sur les logs d'**Apache**, bannissez également les gens qui échouent à se connecter sur l'interface trois fois.
 - Ajouter dans le `bashrc` de root les 3 dernières connexions en tant que root qui sont advenues sur le système.

[spoiler]
A rédiger
[/spoiler]

## Final
Et voilà ! Désolé, cette fois-ci, je ne vous demanderais pas de recommencer plusieurs fois cette partie. Je ne compte pas la mettre dans le **TP évalué** final.

Merci de votre patience, d'avoir apprécié ce cours, n'hésitez pas à le partager en dehors de la formation, ou même à vous le mettre en favori pour plus tard.

@Bientôt !



