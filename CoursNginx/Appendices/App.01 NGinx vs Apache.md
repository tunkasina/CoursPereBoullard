# Appendice 1 : NGinx vs Apache

Si vous avez fait le TP Apache avant celui-ci, vous avez forcement remarque des différences. Les voici résumées :

| Critere | Apache | NGinx |
|---------|--------|-------|
| Modele | Process/thread par connexion | Event-driven asynchrone |
| Memoire | Elevee sous forte charge | Faible, constant |
| Performances statiques | Bonnes | Excellent |
| Dynamic content | mod_php integre | PHP-FPM (externe) |
| Configuration | .htaccess (per-dir) | Centralisee (pas de .htaccess) |
| Commandes | a2ensite, a2dissite, a2enmod | Liens symboliques manuels / conf.d |
| Repertoire | /etc/apache2/ | /etc/nginx/ |
| C10K | Non natif | Concu pour ca |

### Quand utiliser quoi ?
- **Apache** : quand on a besoin de .htaccess (hebergement mutualise), ou qu'on veut du mod_php sans se prendre la tete
- **NGinx** : quand on veut des perfs, un reverse proxy, ou qu'on a beaucoup de trafic statique

### Sources du tableau
Les affirmations du tableau ci-dessus (modele, memoire, C10K, module...) s'appuient sur des sources publiques :

- **Articular C10K de Dan Kegel (1999)**, a l'origine du terme : [The C10K problem](http://www.kegel.com/c10k.html)
- **Documentation officielle NGinx** :
  - `worker_processes` et `worker_connections` : [ngx_core_module](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)
  - Architecture et event loop : [NGinx : une introduction](https://nginx.org/en/docs/beginners_guide.html)
- **Documentation officielle Apache** :
  - `MPM` et modes de gestion des connexions : [Apache MPM](https://httpd.apache.org/docs/2.4/mpm.html)
  - `.htaccess` et configuration par annuaire : [Apache filtre .htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html)
- **Comparatif pratique** : [How NGinx is built for performance (NGINX blog)](https://www.nginx.com/blog/inside-nginx-design-features-for-optimal-performance/)
- **FAQ et benchmarks** : [NGINX vs Apache : analyses et comparatifs (NGINX blog)](https://www.nginx.com/blog/nginx-vs-apache-our-view/)

Pour approfondir la notion de **worker_connections** et la tenue en charge, la doc NGinx recommande de dimensionner `worker_rlimit_nofile` et `worker_connections` ensemble, voir [ce guide](https://www.nginx.com/blog/tuning-nginx/).

Retour aux [chapitres](../Sommaire%20Nginx.md).
