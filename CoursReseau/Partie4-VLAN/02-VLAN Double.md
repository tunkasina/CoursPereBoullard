→ Sur **SW1** : Poste 1 = F0/1, Poste 3 = F0/3  
→ Sur **SW2** : Poste 2 = F0/1, Poste 4 = F0/3  
→ Liaison trunk = F0/24 entre les deux switches.

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

La logique :
 - mode dynamic : autonégocié
 - mode access  1 VLAN
 - mode trunk
	 - Un trunk standard (802.1Q) est **un port physique taggué pour tous les VLANs**.

