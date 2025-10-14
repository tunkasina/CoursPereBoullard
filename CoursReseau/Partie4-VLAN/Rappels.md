
Un **VLAN sépare le trafic au niveau de la couche 2** (liaison) : chaque VLAN est un **domaine de broadcast isolé**. Les trames envoyées dans un VLAN ne sortent jamais vers un autre VLAN sans routage.

- Chaque VLAN a besoin d’un **réseau IP différent** pour que les machines puissent communiquer.
- Si deux VLAN ont le **même réseau IP**, le switch ou le routeur ne sait pas où envoyer les paquets, car la correspondance IP → VLAN devient ambiguë.

Donc le VLAN sépare les interfaces **physiquement/logiquement**, mais **IP = plan d’adressage**, et chaque VLAN doit avoir son propre plan IP.

Attention donc a ne pas mettre les mêmes plan IP de chaque côté

- Les trames **ne sortent pas du VLAN** : une machine dans VLAN A ne verra jamais directement une machine dans VLAN B, donc la communication échoue.
- Si tu essayes de forcer le routage avec la même IP sur deux VLANs, le routeur ou switch L3 sera **confus**, car il ne sait pas quelle interface utiliser pour cette IP. Ça peut provoquer des **conflits IP, des paquets perdus ou des boucles**.