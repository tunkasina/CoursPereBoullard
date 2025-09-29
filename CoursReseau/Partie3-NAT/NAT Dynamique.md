
Configuration R1 (côté privé1 et public1) :
```
! Interface LAN privé 1
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown

! Interface vers R2 (public1)
interface FastEthernet0/1
 ip address 10.0.0.1 255.255.255.252
 ip nat outside
 no shutdown

! NAT overload (PAT) pour les clients du LAN privé 1
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface FastEthernet0/1 overload

! Routage
ip route 0.0.0.0 0.0.0.0 10.0.0.2

```

Configuration R3 (côté privé2 et public2) :
```
! Interface LAN privé 2
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
 no shutdown

! Interface vers R2 (public2)
interface FastEthernet0/1
 ip address 10.0.1.1 255.255.255.252
 ip nat outside
 no shutdown

! NAT overload (PAT) pour les clients du LAN privé 2
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 interface FastEthernet0/1 overload

! Routage
ip route 0.0.0.0 0.0.0.0 10.0.1.2

```

Configuration R2 (routeur central reliant public1 et public2) :
```
! Interface vers R1
interface FastEthernet0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown

! Interface vers R3
interface FastEthernet0/1
 ip address 10.0.1.2 255.255.255.252
 no shutdown

! Routage statique vers LAN privé1 et privé2
ip route 192.168.1.0 255.255.255.0 10.0.0.1
ip route 192.168.1.0 255.255.255.0 10.0.1.1

```

