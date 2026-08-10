# Appendice 5: fail2ban
Rendez-vous dans le répertoire de configuration de Fail2Ban : `/etc/fail2ban`. Faites un `ls` pour voir ce qui s’y trouve. On va détailler chaque élément important pour comprendre le fonctionnement de ce service.

#### fail2ban.conf
C’est le fichier principal de configuration globale. Il définit le comportement général de Fail2Ban, comme le journal à utiliser, la durée des bannissements par défaut et l’emplacement des fichiers de log. On ne modifie généralement pas ce fichier directement, mais on s’en sert comme base.

#### jail.conf et jail.d/
Les "jails" sont le cœur de Fail2Ban. Chaque jail définit :
- Quel **service surveiller** (SSH, Apache, vsftpd…)
- Quel **filtre utiliser** pour détecter les tentatives d’intrusion
- Quelle **action appliquer** (bloquer l’IP, envoyer un mail…)
- Quelle **durée de bannissement**
- `jail.conf` contient les jails par défaut.
- `jail.d/` contient des fichiers `.conf` ou `.local` supplémentaires pour surcharger ou ajouter des jails spécifiques.

#### filter.d/
Ce répertoire contient les **expressions régulières** (filtres) utilisées pour détecter les tentatives de connexion suspectes dans les logs des services. Chaque service surveillé a son fichier, par exemple `sshd.conf` pour **SSH**. Il en existe des tonnes par défaut ! Mais si vous vous en server vous **devez** les lire pour valider ou non le comportement.

#### action.d/
Ici se trouvent les **actions prédéfinies**, comme bloquer une IP via `iptables` ou `firewalld`, envoyer un email d’alerte, ou exécuter un script personnalisé. Chaque action peut être appelée par un jail selon la configuration.

#### fail2ban.log
C’est le journal de Fail2Ban. Il enregistre toutes les activités : IP bannies, jails activés ou désactivés, erreurs de configuration, etc. C’est la première source à consulter pour diagnostiquer un problème.

### Activer et désactiver les jails
Fail2Ban offre plusieurs commandes utiles :
- `fail2ban-client status` → affiche l’état général de Fail2Ban et des jails actives.
- `fail2ban-client status nom_jail` → détaille les IP bannies et l’état du jail ciblé.
- Pour activer un jail spécifique, on l’ajoute dans un fichier `.local` et on recharge Fail2Ban.
- Pour désactiver un jail, on le commente ou le supprime du `.local` et on recharge le service.

<div class="astuce">Après toute modification, rechargez Fail2Ban.</div>


