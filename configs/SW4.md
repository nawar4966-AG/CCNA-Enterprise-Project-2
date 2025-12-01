en
conf t
hostname SW4
vtp mode server
vtp domain scal
vtp password 1234
vlan 130
name m
vlan 140
name n
vlan 150
name o
vlan 160
name p
ex
interface range f0/1 - 3
switchport mode trunk 
switchport trunk allowed vlan 130,140,150,160
ex
spanning-tree mode pvst
ex
wr
