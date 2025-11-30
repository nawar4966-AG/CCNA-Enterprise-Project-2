hostname SW3.1
vtp mode client
vtp domain scal3
vtp password 1234
interface range f0/5 - 8
switchport mode trunk 
channel-group 1 mode passive
ex
interface range f0/1 - 2
switchport access vlan 90
interface range f0/3 - 4
switchport access vlan 100
ex
ex
wr