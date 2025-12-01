en
conf t
hostname SW4.1
vtp mode client
vtp domain scal
vtp password 1234
interface range f0/1 - 2
switchport access vlan 130
interface range f0/3 - 4
switchport access vlan 140
ex
interface range f0/5 - 6
switchport mode trunk 
switchport trunk allowed vlan 130,140,150,160
ex
spanning-tree mode pvst 
spanning-tree vlan 130,140 root primary 
spanning-tree vlan 150,160 root secondary 
ex
wr
