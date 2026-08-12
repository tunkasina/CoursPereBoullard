---
title: "Appendice 3 : Les location blocks"
parent: "TP Nginx"
nav_order: 23
---
# Appendice 3 : Les location blocks

Les `location` blocks sont l'equivalent des `<Directory>` et `<Location>` d'Apache, mais avec leur propre logique de priorite.

### Syntaxe
```nginx
location [ = | ~ | ~* | ^~ ] uri { ... }
```

| Modificateur | Description | Exemple |
|-------------|-------------|---------|
| (rien) | Prefixe standard | `/images/` |
| `=` | Correspondance exacte | `= /favicon.ico` |
| `^~` | Prefixe prioritaire (passe devant les regex) | `^~ /static/` |
| `~` | Regex sensible a la casse | `~ \.php$` |
| `~*` | Regex insensible a la casse | `~* \.(jpg|png)$` |

### Ordre de resolution
1. `= ` (correspondance exacte) : priorite maximale
2. `^~ ` (prefixe prioritaire) : si match, on s'arrete la
3. `~` ou `~*` (regex) : premier match dans l'ordre d'apparition
4. `` (prefixe simple) : le plus long match gagne

### Exemples pratiques
```nginx
# Exact match : priorite 1
location = /favicon.ico {
    log_not_found off;
}

# Prefix prioritaire : passe devant les regex
location ^~ /static/ {
    expires 30d;
}

# Regex : bloque les fichiers caches
location ~ /\. {
    deny all;
}

# Prefixe simple : defaut
location / {
    try_files $uri $uri/ =404;
}
```

Retour aux [chapitres](../Sommaire%20Nginx.md).
