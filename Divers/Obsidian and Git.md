____
1. Vérfier la clé SSH chargée : `ssh-add -l`. Si elle apparait pas, l'ajouter : `ssh-add /chemin/vers/cle_ssh`
2. Créer le répertoire qui servira de dépôt _GIT_ **et** de vault _obsidian_
3. Aller dedans en ligne de commande
4. Initialiser le dépôt sous _GIT_ :
```
git init
git remote add origin git@github.com:[NOM USER GITHUB]/[NOM DEPOT].git
git pull origin main --allow-unrelated-histories
git branch -M main
```
6. Démarrer Obsidian --> "open folder as vault" --> Choisir le répertoire du dépot
7. Installer le plugin _GIT_ de Vinzent (Denis Olehov)
8. Cliquez à gauche "Open Git source control"
9. Dans le menu à droite lancer un _push_ choisir _origin_, _origin/main_ etc. vous connaissez
10. Aller dans les paramètres du plugin et faire les réglages d'autocomit :
	1. Auto commit-and-sync interval (minutes) mettre quelque chose (moi j'ai mis 1)
	2. Activer le Auto commit-and-sync after stopping file edits
	3. Parcourir les autres options au cas où
11. Checker quand meme sur Github que votre dépot a des mises à jour