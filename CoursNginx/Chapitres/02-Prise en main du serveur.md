# Prise en main du serveur
Dans le cadre de votre mise en production, vous allez installer NGinx sur une VM Debian fraiche. Quand on loue un serveur "sans option" chez un hebergeur, c'est exactement ce que vous avez : un OS fraichement installe, vide de logiciels, et c'est a vous de tout monter.

<div class="astuce">Faites un snapshot !</div>

Mais avant de foncer tet baissee dans l'installation, prenez cinq minutes pour explorer votre serveur. Vous devez savoir sur quoi vous mettez les pieds.

Enfin, prenez des _notes_ de telle facon que _vous soyez capable de tout refaire sans mon support_ !

## Consignes
Evidemment, vous chercherez **par vous meme et par tous les moyens necessaires**, les commandes a taper pour faire chacune de ces actions ! Ne vous jetez pas sur la solution, vous _savez_ que votre cerveau ne memorisera rien dans ce cas.

### Prise en main
Vous ne savez pas comment faire ? C'est normal, mais le rester sans rien faire, non ! Internet, les amis, votre prof, cherchez, _cherchez_, **cherchez** !
 - Trouvez la version et le niveau de mise a jour de votre serveur
 - Mettez a jour votre serveur
 - Trouvez quelle est l'adresse IP de votre serveur
 - Verifiez que vous etes bien sur un serveur vierge (pas d'Apache, pas de NGinx deja installe)

[spoiler]
 - `lsb_release -a` + a la connexion, les infos de version du noyau sont affichees
 - `apt update && apt upgrade` ...mise a jour
 - `ip a` pour trouver votre IP
 - `systemctl status apache2 2>/dev/null && echo "Apache present !" || echo "Pas d'Apache"` : verifier qu'Apache n'est pas la
 - `systemctl status nginx 2>/dev/null && echo "NGinx present !" || echo "Pas de NGinx"` : ni NGinx d'ailleurs
[/spoiler]

### Installer NGinx
NGinx n'est pas installe par defaut sur Debian. Il va falloir corriger ca.
 - Installez NGinx
 - Verifiez que le demon fonctionne
 - Comprenez les違い entre les commandes `start`, `stop`, `reload` et `restart`
 - Testez que le serveur web repond

[spoiler]
 - `apt update && apt install -y nginx`
 - `systemctl status nginx` pour voir si tout va bien
 - `ss -tlnp | grep 80` pour verifier qu'il ecoute
 - `curl -I http://localhost` pour voir la reponse HTTP
 - La difference entre `reload` (recharge la config sans couper les connexions) et `restart` (coupe tout et redemarre) est fondamentale. En production, on **reload** presque toujours.
[/spoiler]

### Explorer l'arborescence de NGinx
Apache a sa propre facon de ranger les choses. NGinx aussi, mais c'est different. Il va falloir comprendre ou se trouve quoi.
 - Trouvez le fichier de configuration principal
 - Trouvez les dossiers `sites-available` et `sites-enabled` et expliquez la difference
 - Trouvez les logs
 - Lisez le fichier de configuration principal et comprenez sa structure

[spoiler]
NGinx a une arborescence assez propre :
 - `/etc/nginx/nginx.conf` : le fichier principal, point d'entree de toute la config
 - `/etc/nginx/sites-available/` : les configurations de sites disponibles
 - `/etc/nginx/sites-enabled/` : les configurations actives (liens symboliques vers `sites-available`)
 - `/var/log/nginx/access.log` : logs d'acces
 - `/var/log/nginx/error.log` : logs d'erreur

La structure du `nginx.conf` est hierarchique :
```
main context (global)
├── events { }       # gestion des connexions
└── http { }         # tout le trafic HTTP
    ├── server { }   # virtual host (= site)
    └── upstream { } # groupe de serveurs backend
```

A la difference d'Apache qui utilise des fichiers `*.conf` separes et des commandes `a2ensite`/`a2dissite`, NGinx fonctionne avec des liens symboliques entre `sites-available` et `sites-enabled`. Vous allez devoir creer vos liens a la main. Pas de `a2ensite` ici, on fait du sport.

Pour tester la syntaxe d'une configuration : `nginx -t`. C'est votre meilleur ami. Utilisez-le avant chaque reload.
[/spoiler]

### Premier site statique
On va preparer le terrain pour la suite. Notre projet fil-rouge s'appelle **Grafana Project**, et il faut lui faire une page d'accueil.
 - Creez un repertoire `/var/www/grafana-project/`
 - Ajoutez-y une page `index.html` minimaliste avec un titre et un paragraphe
 - Creez un sous-dossier `/var/www/grafana-project/static/` avec un fichier CSS basique

[spoiler]
```bash
mkdir -p /var/www/grafana-project/static/css

cat > /var/www/grafana-project/index.html << 'EOF'
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Projet Grafana</title>
  <link rel="stylesheet" href="/static/css/style.css">
</head>
<body>
  <h1>Projet Grafana</h1>
  <p>Ce serveur NGinx sert de reverse proxy pour Grafana.</p>
</body>
</html>
EOF

cat > /var/www/grafana-project/static/css/style.css << 'CSS'
body { font-family: sans-serif; max-width: 800px; margin: 2em auto; }
h1 { color: #f05a28; }
CSS
```
[/spoiler]

### Configurer le VirtualHost
Pour qu'Apache serve ce site, on aurait cree un fichier dans `sites-available` et fait un `a2ensite`. Pour NGinx, c'est le meme principe... mais sans la commande magique.
 - Creez un fichier de configuration dans `/etc/nginx/sites-available/grafana-project`
 - Configurez-le pour ecouter sur le port 80, avec `server_name` a `_` (catch-all)
 - La racine doit etre `/var/www/grafana-project`
 - Desactivez le site par defaut
 - Activez votre site
 - Testez la configuration et rechargez NGinx
 - Testez avec `curl`

[spoiler]
```nginx
# /etc/nginx/sites-available/grafana-project
server {
    listen 80;
    server_name _;

    root /var/www/grafana-project;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Commandes :
```bash
ln -s /etc/nginx/sites-available/grafana-project /etc/nginx/sites-enabled/
rm /etc/nginx/sites-enabled/default   # supprimer le default
nginx -t                              # tester la syntaxe
systemctl reload nginx                # appliquer
curl -I http://localhost/             # tester
```
[/spoiler]

### Comprendre les locations
Rappelez-vous, dans le TP Apache, vous aviez des `<Directory>` et des `<Location>`. NGinx a les `location` blocks, qui fonctionnent differemment.
 - Ajoutez une location `= /favicon.ico` qui coupe les logs
 - Ajoutez une location `~ /\.` qui refuse l'acces aux fichiers caches (`.git`, `.env`, etc.)
 - Ajoutez une location `~ \.php$` qui retourne 403 (pas de PHP pour l'instant)
 - Testez chaque cas

[spoiler]
```nginx
server {
    location = /favicon.ico {
        log_not_found off;
        access_log off;
    }

    location ~ /\. {
        deny all;
        return 403;
    }

    location ~ \.php$ {
        deny all;
        return 403;
    }
}
```

Test :
```bash
curl -I http://localhost/favicon.ico
curl -I http://localhost/.env          # doit donner 403
curl -I http/localhost/test.php        # doit donner 403
```

Rappel priorite des locations :
1. `= ` (exact) : priorite maximale
2. `^~ ` (prefix prioritaire)
3. `~` ou `~*` (regex, dans l'ordre d'apparition)
4. `` (prefix simple, defaut)
[/spoiler]

## Final
Vous avez maintenant un serveur NGinx fonctionnel qui sert un site statique. C'est deja pas mal, mais ce n'est que le debut.

Prenez vos notes. **Restaurez votre snapshot**, et recommencez sans aucune aide.
Si vous n'y arrivez pas, c'est qu'il manque des choses dans vos notes. Il faut alors suivre a nouveau cette page et **mieux documenter** les soucis que vous avez rencontres. Obstinez-vous, vous _devez_ etre capable de faire tout ca !

[03-Reverse Proxy et Grafana](./03-Reverse%20Proxy%20et%20Grafana.md)
