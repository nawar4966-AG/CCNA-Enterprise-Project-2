hostname SW1.2
vtp mode client
vtp domain scal
vtp password 1234
interface range f0/5 - 8
switchport mode trunk 
channel-group 2 mode passive
ex
interface range f0/1 - 2
switchport access vlan 30
interface range f0/3 - 4
switchport access vlan 40
ex
ex
wr