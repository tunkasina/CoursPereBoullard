Tout sur un seul SW

```
enable
configure terminal

vlan 10
name VLAN_ADMIN
exit

vlan 20
name VLAN_USERS
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

/!\ Chaque VLAN a sa propre plage IP



