___
Reproduisez le plan dans la salle.
``` mermaid
graph TB

R1["R1"]
R2["R2"]
R3["R3"]
R4["R4"]

subgraph Network_A["192.168.192.224/0xFFFFFFE0"]
  PC_A1["PCs row A (DHCP)"]
end

subgraph Network_B["192.168.10.0"]
  PC_B1["PCs row B (DHCP)"]
end

subgraph Network_C["192.168.11.0"]
  PC_C1["PCs row C (DHCP)"]
end

subgraph Network_D["192.168.192.64/0xFFFFFFE0"]
  PC_D1["PCs row D (DHCP)"]
end

%% Liaisons Routeurs ↔ Networks
R1 --- Network_A
R4 --- Network_A

R1 --- Network_B
R2 --- Network_B

R2 --- Network_C
R3 --- Network_C

R3 --- Network_D
R4 --- Network_D

%% Liaisons PC ↔ Networks
PC_A1 --- Network_A
PC_B1 --- Network_B
PC_C1 --- Network_C
PC_D1 --- Network_D

```
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
