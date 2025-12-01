en
conf t
hostname SW2
vtp mode server
vtp domain scal
vtp password 1234
vlan 50 
name e
vlan 60
name f
vlan 70
name g
vlan 80
name h
ex
interface range f0/1 - 3
switchport mode trunk 
switchport trunk allowed vlan 50,60,70,80
ex
spanning-tree mode pvst 
ex
wr
