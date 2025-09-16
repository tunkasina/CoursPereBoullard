#### Sous Windows
##### Créer la clé
 - cf [[Clé SSH]]
##### Ajoutez la clé au client
 - Ajouter la clé à votre agent SSH `ssh-add [Nom de la clé]` (depuis le répertoire où il y a la clé en question bien sûr)
	 - Si cela échoue, démarrez votre service nécessaire (en Admin) :
		 - `Get-Service ssh-agent | Set-Service -StartupType Automatic`
		 - `Start-Service ssh-agent`
##### Ajouter la clé au serveur
 - Sur [GitHub](https://github.com/settings/keys), cliquez sur "New SSH Key"
 - Copiez collez le contenu de votre fichier `*.pub`
 - Testez avec `ssh -T git@github.com`, il devrait vous répondre avec `Hi [Pseudo Github]! You've successfully authenticated, but GitHub does not provide shell access.`
#### Sous Linux
Lisez la doc windows, si vous êtes malin, c'est encore plus facile sous linux.
