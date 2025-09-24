____
## Consignes
 - Reprendre le réseau de [[Routage Dynamique]]
 - Faire comprendre comment un hôte peut recevoir un **message ICMP Redirect** quand un routeur détecte qu’un autre routeur est mieux placé pour atteindre une destination.
 - Les étudiants doivent **trouver une situation où un PC envoie un paquet vers une passerelle qui n’est pas la meilleure** pour atteindre une autre sous-réseau.
## Etape 1 : Mettre au point l'expérimentation
 - Les étudiants doivent trouver une manipulation qui illustre le redirect avec RIP
> [!soluce]- Par exemple ...
>  - Entrer une route statique fausse sur un PC (mauvaise passerelle) `ip route add 192.168.192.64/27 via 192.168.192.225`
> - Configurer deux routes par défaut sur un PC et observer quel routeur envoie le Redirect.
> - Placer un PC dans un VLAN non optimal pour atteindre un réseau et voir le Redirect.
> - Ajouter une route statique incorrecte sur un PC vers un réseau connu pour déclencher le Redirect.
> - Simuler un lien temporairement coupé et voir le Redirect rétablir le chemin optimal.
> - Créer un chemin à plusieurs routeurs et forcer un PC à utiliser une passerelle sous-optimale pour observer le Redirect.
> 

## Etape 2 : Observer et expliquer
 - Vérifier l'échange de paquets *icmp*.
> [!soluce]- Procédure
> - Faire un ping vers une IP d'un réseau distant concerné
> - Observer dans Wireshark (filtre `icmp && ip.src == <IP_PC>`) et voir les messages des routeurs intermédiaires. 
> - Vérifier la table de routage du PC après les échanges `ip route`
