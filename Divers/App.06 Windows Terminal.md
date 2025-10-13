# Appendice 6: Windows Terminal
Alors comme ça on décide d'utiliser Windows pour accéder à son serveur, hmmmm ? N'importe quoi.

### Télécharger l'exécutable
Rendez vous sur https://github.com/microsoft/terminal/releases et récupérez une version **x64** au format **zip**. Au moment ou j'écrivais ces lignes la dernière était : 
https://github.com/microsoft/terminal/releases/download/v1.23.12371.0/Microsoft.WindowsTerminal_1.23.12371.0_x64.zip

Décompressez la n'importe où, comme `Z:\windozterm`
Testez le en double-cliquant sur `WindowsTerminal.exe`.

### Et pour SSH du coup ?
Et bien faites la même procédure qu'avec un Linux ! Il faut mettre sa clé dans son `.ssh` dans son **home**.

Pour aller dans son **home** :
 - en DOS : `cd %HOMEPATH%`
 - en PowerShell : `cd $HOME` ou `cd ~`

Copier la clé de votre serveur à votre poste ainsi, depuis votre **home** :
 -: `scp webadmin@172.22.69.238:/home/webadmin/[NOM_EXPLICITE] ./.ssh/`
