___
## Consignes
 - Reproduisez le plan dans la salle.
![[Pasted image 20250924150309.png]]

## Etape 1 : Compléter le plan d'adressage
> [!soluce]- Plan complet
> |Réseau|R1|R2|R3|R4|
> |---|---|---|---|---|
> |192.168.192.224/27 (A)|.225|–|–|.226|
> |192.168.10.0/24 (B)|.1|.2|–|–|
> |192.168.11.0/24 (C)|–|.1|.2|–|
> |192.168.192.64/27 (D)|–|–|.65|.66|

## Etape 2 : Configurer les interfaces
> [!soluce]- Commandes
> Par exemple, pour R1:
> ``` cisco
enable
configure terminal
interface FastEthernet0/0
>  description to_A_row
 ip address 192.168.192.225 255.255.255.224
 no shutdown
exit
interface FastEthernet0/1
>  description to_B_row
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
end
write memory 
> ```
> *Répéter pour R2, R3, R4 avec les IP correspondantes.*

 - Vérification :
``` bash
show ip interface brief
show ip route
```

## Etape 3 : Ajouter les routes statiques
> [!soluce]- Commandes
> Par exemple, pour R1 :
> ``` cisco
configure terminal
ip route 192.168.11.0 255.255.255.0 192.168.10.2
ip route 192.168.192.64 255.255.255.224 192.168.10.2
end
write memory
> ```
> *Répéter pour R2, R3, R4 avec les IP correspondantes.*

## Etape 4 : Vérifications
- Ping entre PC de différentes rangées pour vérifier que tout communique
- Valider avec `show ip interface brief` et `show ip route` sur les postes que les chemins existent.

Dans Wireshark (PC) : filtre `icmp`. On observe les échos traverser les routeurs.
