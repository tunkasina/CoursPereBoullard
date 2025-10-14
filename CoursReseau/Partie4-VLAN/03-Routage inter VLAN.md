

Sur le routeur :

```
enable
configure terminal

interface gigabitEthernet0/0
no shutdown

interface gigabitEthernet0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface gigabitEthernet0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0

end
write memory

```

SW 1 :

```
enable
configure terminal

vlan 10
name VLAN_A
vlan 20
name VLAN_B

interface fastEthernet0/24
switchport mode trunk
switchport trunk allowed vlan 10,20
exit

interface range fastEthernet0/1 - 2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet0/3 - 4
switchport mode access
switchport access vlan 20
exit

end
write memory

```

SW 2 : ne change pas par rapport au TP 2



