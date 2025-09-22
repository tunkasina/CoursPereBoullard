
- le **client possède la clé privée** → il signe un challenge
- le **serveur connaît la clé publique** → il vérifie la signature
##### Créer la clé
 - Aller dans `C:\Users\[NOM]\.ssh`
 - Créer une clé `ssh-keygen -t ed25519 -C "[Nom qui précise qui utilise la clé]"`
 - Il demande un nom de fichier, mettez quelque chose de mnémotechnique
##### Ajoutez la clé au client
 - Ajouter la clé à votre agent SSH `ssh-add [Nom de la clé]` (depuis le répertoire où il y a la clé en question bien sûr)
	 - Si cela échoue, démarrez votre service nécessaire (en Admin) :
		 - `Get-Service ssh-agent | Set-Service -StartupType Automatic`
		 - `Start-Service ssh-agent`
##### Ajouter la clé au serveur
 - sous .ssh `authorized_keys`
#### Sous Linux
Lisez la doc windows, si vous êtes malin, c'est encore plus facile sous linux.
