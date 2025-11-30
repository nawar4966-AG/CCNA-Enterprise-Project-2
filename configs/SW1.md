hostname SW1
vtp mode server
vtp domain scal
vtp password 1234
vlan 10 
name a
vlan 20
name b
vlan 30
name c
vlan 40
name d
ex
interface range f0/9
switchport access vlan 10
interface range f0/10
switchport mode trunk 
interface range f0/1 - 4
channel-group 1 mode active 
ex
interface range f0/5 - 8
channel-group 2 mode active 
ex
ex
wr