
Pour un NAT complet :
`ip nat inside source static 192.168.1.2 10.0.0.1`

Pour limiter et préciser le protocole :
`ip nat inside source static tcp 192.168.1.2 80 10.0.0.1 80`

R1 (privé1 → public1)
```
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown

interface FastEthernet0/1
 ip address 10.0.0.1 255.255.255.252
 ip nat outside
 no shutdown

! NAT statique : machine 192.168.1.2 ↔ 10.0.0.1
ip nat inside source static 192.168.1.2 10.0.0.1

! Routage par défaut
ip route 0.0.0.0 0.0.0.0 10.0.0.2
```

R3 (privé2 → public2)
```
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown

interface FastEthernet0/1
 ip address 10.0.1.1 255.255.255.252
 ip nat outside
 no shutdown

! NAT statique : machine 192.168.1.2 ↔ 10.0.1.1
ip nat inside source static 192.168.1.2 10.0.1.1

! Routage par défaut
ip route 0.0.0.0 0.0.0.0 10.0.1.2

```

R2 (routeur central)
```
interface FastEthernet0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

interface FastEthernet0/1
 ip address 10.0.1.2 255.255.255.252
 no shutdown

! Routes statiques pour atteindre les LAN derrière R1 et R3
ip route 192.168.1.0 255.255.255.0 10.0.0.1
ip route 192.168.1.0 255.255.255.0 10.0.1.1

```