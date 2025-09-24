__________
## Rappel sur le routage
- Principe du routage : aiguiller les paquets vers le bon réseau de destination.
- Protocoles concernés : RIP, OSPF, ICMP.
- Table de routage : contient les réseaux connus, leur passerelle et l’interface de sortie.

## Mise en situation (intro)

« Bonjour. La semaine dernière vous avez mis en place le service DHCP : chaque hôte reçoit automatiquement une adresse IP, un masque, une passerelle et DNS. Aujourd’hui on garde ce réseau dynamique pour voir comment les routeurs apprennent où envoyer les paquets. Objectif : vérifier que vos clients obtiennent bien une IP via DHCP avant de toucher aux routes. »

« Aujourd’hui on révise le routage IP (principe, RIP, OSPF et ICMP). Trois TP :
- TP0 : routage statique manuel sur Cisco avec capture Wireshark.
- TP1 : routage dynamique en RIP et observation des échanges.
- TP2 : expérimentation d’un ICMP Redirect.  
    But : comprendre la construction et la modification des tables de routage, voir les paquets en réel, et reproduire les manipulations sur Cisco. »

## Réseau à mettre en place
1. 192.168.192.224/27 : R4 ↔ R1
2. 192.168.10.0/24 : R1 ↔ R2
3. 192.168.11.0/24 : R2 ↔ R3
4. 192.168.192.64/27 : R3 ↔ R4

Rangées de PC :
- A = 192.168.192.224/27
- B = 192.168.10.0/24
- C = 192.168.11.0/24
- D = 192.168.192.64/27

Matériel en salle :
- 14 PC (4 rangées A–D)
- Baie ouest + baie est
- Chaque baie : 2× Switch 2960, 2× Routeur 1841, 1× Routeur 2800

Chaque rangée est reliée à un DHCP logiciel qui distribue les adresses.

## Les TP de la semaine :
 - [[Routage Statique]]
 - [[Routage Dynamique]]
 - [[ICMP Redirect]]

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
R3 --- Réseau_C

R3 --- Réseau_D
R4 --- Réseau_D

%% Liaisons PC ↔ Networks
PC_A1 --- Réseau_A
PC_B1 --- Réseau_B
PC_C1 --- Réseau_C
PC_D1 --- Réseau_D

```
