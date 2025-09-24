___
## Consignes
 - Reprendre le réseau de [[Routage Statique]]
 - Le passer en routage dynamique avec RIP

## Etape 1 : Suppression des routes statiques
 - Sur chaque routeur, supprimer toutes les routes statiques configurées.
> [!soluce]- Commandes
> ``` cisco
enable
configure terminal
no ip route 192.168.11.0 255.255.255.0 192.168.10.2
no ip route 192.168.192.64 255.255.255.224 192.168.10.2
end
write memory
> ```
> *Répéter pour R2, R3, R4 avec les IP correspondantes.*

## Etape 2 : Activation de RIP
 - Sur chaque routeur, activer RIP version 2 et inclure les réseaux locaux connectés :
> [!soluce]- Commandes
> ``` cisco
enable
configure terminal
router rip
 version 2
 network 192.168.192.0
 network 192.168.10.0
 no auto-summary
end
write memory
> ```
> *Répéter pour R2, R3, R4 avec les IP correspondantes.*


