
```
+---+----+----------PC1
|   |    +----------PC2
|   R1
|   | 
R3  +----+----------PC3
|   |    +----------PC4
|   R2
|   |    +----------PC5
+---+----+----------PC6
```

- **R1** :
    - Fa0/0 → réseau 192.168.2.0/24 : 192.168.2.254
    - Fa0/1 → réseau 192.168.3.0/24 : 192.168.3.253
- **R2** :
    - Fa0/0 → réseau 192.168.3.0/24 : 192.168.3.253
    - Fa0/1 → réseau 192.168.4.0/24 : 192.168.4.254
- **R3** :
    - Fa0/0 → réseau 192.168.2.0/24 : 192.168.2.253
    - Fa0/1 → réseau 192.168.4.0/24 : 192.168.4.253

R1
```
enable
configure terminal

interface fastEthernet0/0
ip address 192.168.2.254 255.255.255.0
no shutdown

interface fastEthernet0/1
ip address 192.168.3.253 255.255.255.0
no shutdown

ip route 192.168.4.0 255.255.255.0 192.168.3.253
end
write memory

```

R2
```
enable
configure terminal

interface fastEthernet0/0
ip address 192.168.3.253 255.255.255.0
no shutdown

interface fastEthernet0/1
ip address 192.168.4.254 255.255.255.0
no shutdown

ip route 192.168.2.0 255.255.255.0 192.168.3.253
end
write memory

```

R3 
```
enable
configure terminal

interface fastEthernet0/0
ip address 192.168.2.253 255.255.255.0
no shutdown

interface fastEthernet0/1
ip address 192.168.4.253 255.255.255.0
no shutdown

ip route 192.168.3.0 255.255.255.0 192.168.2.254
end
write memory

```

Les **PC** : passerelle = IP du routeur côté réseau du PC.
Exemple : PC1 → passerelle 192.168.2.254, PC5 → passerelle 192.168.4.254.