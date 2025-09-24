__________
## Rappel sur le routage
- Principe du routage : aiguiller les paquets vers le bon réseau de destination.
- Protocoles concernés : RIP, OSPF, ICMP.
- Table de routage : contient les réseaux connus, leur passerelle et l’interface de sortie.

## Mise en situation (intro)
« Bonjour. La semaine dernière vous avez mis en place le service DHCP : chaque hôte reçoit automatiquement une adresse IP, un masque, une passerelle et DNS. Aujourd’hui on garde ce réseau dynamique pour voir comment les routeurs apprennent où envoyer les paquets. Objectif : vérifier que vos clients obtiennent bien une IP via DHCP avant de toucher aux routes. »

« Aujourd’hui on révise le routage IP (principe, RIP, OSPF et ICMP). Trois TP :
- TP0 : [[Routage Statique]] manuel + observation wireshark
- TP1 : [[Routage Dynamique]] en RIP + observation 
- TP2 : [[ICMP Redirect]] + experimentation
    But : comprendre la construction et la modification des tables de routage, voir les paquets en réel, et reproduire les manipulations sur Cisco. »

``` mermaid
graph TB

R1["R1"]
R2["R2"]
R3["R3"]
R4["R4"]

subgraph Network_A["192.168.192.224/27"]
  PC_A1["PCs row A (DHCP)"]
end

subgraph Network_B["192.168.10.0/24"]
  PC_B1["PCs row B (DHCP)"]
end

subgraph Network_C["192.168.11.0/24"]
  PC_C1["PCs row C (DHCP)"]
end

subgraph Network_D["192.168.192.64/27"]
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
