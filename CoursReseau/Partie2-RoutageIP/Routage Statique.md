___
#### Consignes
 - Reproduisez le plan dans la salle.
![[Pasted image 20250924150309.png]]

#### Etape 1 : Compléter le plan d'adressage
> [!soluce]- Plan complet
> 192.168.192.224/27 : R1 = .225, R4 = .226
> 192.168.10.0/24 : R1 = .1, R2 = .2
> 192.168.11.0/24 : R2 = .1, R3 = .2
> 192.168.192.64/27 : R3 = .65, R4 = .66


### Étape 1 — Plan d’adressage
- 192.168.192.224/27 : R1 = .225, R4 = .226
- 192.168.10.0/24 : R1 = .1, R2 = .2
- 192.168.11.0/24 : R2 = .1, R3 = .2
- 192.168.192.64/27 : R3 = .65, R4 = .66

### Étape 2 — Configuration des interfaces
 - Exemple R1 :
``` cisco
enable
configure terminal
interface FastEthernet0/0
 ip address 192.168.192.225 255.255.255.224
 no shutdown
exit
interface FastEthernet0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
end
write memory
```
*Répéter pour R2, R3, R4 avec les IP correspondantes.*

 - Vérification :
``` bash
show ip interface brief
show ip route
```

### Étape 3 — Ajout des routes statiques
 - Exemple R1 :
``` cisco
configure terminal
ip route 192.168.11.0 255.255.255.0 192.168.10.2
ip route 192.168.192.64 255.255.255.224 192.168.10.2
end
write memory

```
*Répéter pour R2, R3, R4 avec les IP correspondantes.*

 - Vérification :
 ``` bash
ping <IP_d_un_autre_réseau>
show ip route
```

Dans Wireshark (PC) : filtre `icmp`. On observe les échos traverser les routeurs.
