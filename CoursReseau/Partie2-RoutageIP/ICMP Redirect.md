____
## Consignes
 - Reprendre le réseau de [[Routage Dynamique]]
 - Faire comprendre comment un hôte peut recevoir un **message ICMP Redirect** quand un routeur détecte qu’un autre routeur est mieux placé pour atteindre une destination.
 - Les étudiants doivent **trouver une situation où un PC envoie un paquet vers une passerelle qui n’est pas la meilleure** pour atteindre une autre sous-réseau.
## Etape 1 : Mettre au point l'expérimentation
 - Les étudiants doivent trouver une manipulation qui illustre le redirect avec RIP
> [!soluce]- Par exemple ...
>  - Entrer une route statique fausse sur un PC (mauvaise passerelle)
>  - 
> 

## Etape 2 : Observer et expliquer
 - Vérifier l'échange de paquets *icmp*.
> [!soluce]- Procédure
> - Faire un ping vers une IP du réseau distant concerné (par ex. un PC de D).
> - Observer dans Wireshark (filtre `icmp && ip.src == <IP_PC>`).
> - Le PC doit recevoir un **ICMP Redirect** du routeur intermédiaire.
> - Vérifier la table de routage du PC