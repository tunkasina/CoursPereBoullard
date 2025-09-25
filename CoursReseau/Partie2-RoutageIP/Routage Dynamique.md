___
## Consignes
 - Reprendre le réseau de [[Routage Statique]]
 - Le passer en routage dynamique avec RIP

## Etape 1 : Suppression des routes statiques
 - Sur chaque routeur, supprimer toutes les routes statiques configurées.
> [!soluce]- Commandes
> Par exemple, pour R1 :
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
> Par exemple, pour R1 :
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
> 
> (`no auto-summary` garantit que RIP annonce chaque sous-réseau avec son masque exact au lieu de résumer par classe, ce qui évite les erreurs de routage dans les réseaux subnettés.)
> 
> *Répéter pour R2, R3, R4 avec les IP correspondantes.*

## Etape 3 : Vérifications
- Ping entre PC de différentes rangées pour vérifier que RIP a correctement propagé les routes.
- Sur chaque routeur, vérifier que les routes distantes sont automatiquement ajoutées sans configuration statique.

Dans Wireshark (PC) : filtre `udp.port == 520` pour observer les échanges RIP périodiques entre routeurs.

 > [!soluce]- Résultats
>  - `show ip route` pour R1 :
> ``` cisco
R1> enable
R1# show ip route
Codes: C - connected, R - RIP
C    192.168.192.224/27 is directly connected, FastEthernet0/0
C    192.168.10.0/24 is directly connected, FastEthernet0/1
R    192.168.11.0/24 [120/1] via 192.168.10.2, 00:00:12, FastEthernet0/1
R    192.168.192.64/27 [120/1] via 192.168.10.2, 00:00:12, FastEthernet0/1
> ```
> - Les routes statiques ont disparu.
> - Les réseaux distants sont maintenant **appris via RIP** (`R`).
> - `120` = administrative distance RIP par défaut, `1` = métrique.
