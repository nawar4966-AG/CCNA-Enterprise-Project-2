hostname SW4.2
vtp mode client
vtp domain scal4
vtp password 1234
interface range f0/1 - 2
switchport access vlan 150
interface range f0/3 - 4
switchport access vlan 160
ex
interface range f0/5 - 6
switchport mode trunk 
switchport trunk allowed vlan 130,140,150,160
ex
spanning-tree mode pvst 
spanning-tree vlan 130,140 root secondary 
spanning-tree vlan 150,160 root primary  
ex
wr