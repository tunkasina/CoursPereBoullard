
Un **VLAN sépare le trafic au niveau de la couche 2** (liaison) : chaque VLAN est un **domaine de broadcast isolé**. Les trames envoyées dans un VLAN ne sortent jamais vers un autre VLAN sans routage.

- Chaque VLAN a besoin d’un **réseau IP différent** pour que les machines puissent communiquer.
- Si deux VLAN ont le **même réseau IP**, le switch ou le routeur ne sait pas où envoyer les paquets, car la correspondance IP → VLAN devient ambiguë.

Donc le VLAN sépare les interfaces **physiquement/logiquement**, mais **IP = plan d’adressage**, et chaque VLAN doit avoir son propre plan IP.

