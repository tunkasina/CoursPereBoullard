# Appendice 2: droits Nginx
#### Rappel Droits Unix
Les droits Unix (**640**, **777**, etc.) sont basés sur des bits (0 ou 1) autorisant la lecture (r), l'écriture (w) et l'exécution (x).

Chaque bloc de 3 bits définit les droits pour : **Propriétaire**, **Groupe**, et **Autres**.
- `rwxr-xr-x` $\rightarrow$ `111`, `101`, `101` $\rightarrow$ **755**
- `rw-r--r--` $\rightarrow$ `110`, `100`, `100` $\rightarrow$ **644**

<div class="astuce">Il est obligatoire d'avoir les droits d’exécution (x) pour parcourir un répertoire.</div>

#### Et Nginx ?
Sous Debian, **nginx** s’exécute également avec l'utilisateur **www-data**, appartenant au groupe **www-data**.

L'erreur classique est de donner tous les droits à `www-data`. Cela expose le serveur à :
- **RCE via Upload** : Un fichier malveillant uploadé peut être exécuté avec les droits du serveur.
- **Défacement** : Si Nginx peut écrire partout, un attaquant peut modifier vos pages.

On privilégie donc le principe du **moindre privilège**. Deux approches courantes :
- Ignorer `www-data` et configurer en **775** (dossiers) / **664** (fichiers) avec un groupe commun.
- Utiliser le groupe **www-data**, fermer l'accès aux "autres", et configurer en **750** (dossiers) / **640** (fichiers).
