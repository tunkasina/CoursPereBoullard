# Appendice 3: fonctionnement d'apache
Rendez-vous dans le répertoire de configuration d’Apache : `/etc/apache2`. Faites un `ls` pour voir ce qui s’y trouve. On va détailler chaque élément important pour bien comprendre le fonctionnement du serveur.  
#### apache2.conf
C’est le fichier de configuration principal d’Apache. Il orchestre tout : il va charger des fichiers secondaires en fonction des ports, des modules activés et des sites configurés. C’est le point de départ de toute configuration.  
#### Les répertoires `*-available` et `*-enabled`
Apache utilise un modèle simple :  
- Les répertoires `*-available` contiennent des fichiers de configuration qui **sont disponibles**, mais pas forcément activés.  
- Les répertoires `*-enabled` contiennent des **liens symboliques** vers les fichiers de `*-available` qui sont effectivement activés.  

Ce modèle s’applique à plusieurs types de configurations :  
- Les fichiers généraux : `conf-available / conf-enabled`  
- Les sites web : `sites-available / sites-enabled`  
- Les modules (PHP, MySQL…) : `mods-available / mods-enabled`  

#### envvars
Ce fichier définit des variables d’environnement **spécifiques à Apache**. Ces variables ne sont pas globales au système, elles ne servent que pour le fonctionnement du serveur.  
#### magic  
Le fichier `magic` sert à détecter les types MIME des fichiers en examinant leurs premiers octets. Cette détection "magique" est plus fiable que de se baser uniquement sur l’extension du fichier (_c'est pas dur ceci dit_).  
#### ports.conf  
Ici, on définit les ports sur lesquels Apache écoute. Par défaut, on y trouve 80 pour le HTTP et 443 pour le HTTPS. On peut conserver ces ports ou en ajouter d’autres selon les besoins.  
### Activer et désactiver modules et sites
Pour gérer facilement ce qui est activé ou non, Apache fournit deux commandes principales :  
**a2enmod / a2dismod**  
- `a2enmod nom_module` → active un module (ex : ssl, rewrite, headers)  
- `a2dismod nom_module` → désactive un module  

**a2ensite / a2dissite**  
- `a2ensite nom_site` → active un site (crée le lien symbolique dans `sites-enabled`)  
- `a2dissite nom_site` → désactive un site (supprime le lien symbolique)  

Pour lister les éléments disponibles ou activés :  
- Tapez `a2dissite` ou `a2dismod` suivi de `TAB` pour voir ce qui est actif.  
- Pour activer un module ou site, utilisez `a2enmod` ou `a2ensite` suivi du nom.  

<div class="astuce">Après toute modification, rechargez Apache !</div>

