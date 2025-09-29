
Petit rappel rapide des **classes d’adresses IPv4** (historiques, utilisées pour comprendre mais plus vraiment appliquées depuis le CIDR (_# Classless Inter-Domain Routing_)) :
- **Classe A**
    - Plage : `0.0.0.0` → `127.255.255.255`
    - Masque par défaut : `255.0.0.0` (/8)
    - Premier octet : `0–127`
    - Réseaux énormes (16 millions d’hôtes)
- **Classe B**
    - Plage : `128.0.0.0` → `191.255.255.255`
    - Masque par défaut : `255.255.0.0` (/16)
    - Premier octet : `128–191`
    - Taille moyenne (65 536 hôtes)
- **Classe C**
    - Plage : `192.0.0.0` → `223.255.255.255`
    - Masque par défaut : `255.255.255.0` (/24)
    - Premier octet : `192–223`
    - Petits réseaux (254 hôtes)
- **Classe D**
    - Plage : `224.0.0.0` → `239.255.255.255`
    - Pas de masque (utilisé pour multicast)
- **Classe E**
    - Plage : `240.0.0.0` → `255.255.255.255`
    - Réservée à la recherche
---
Les **adresses privées** (non routables sur Internet) :
- Classe A : `10.0.0.0/8`
- Classe B : `172.16.0.0/12`
- Classe C : `192.168.0.0/16`