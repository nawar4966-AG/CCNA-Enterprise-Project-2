en  
conf t  
hostname SW2.2  
vtp mode client  
vtp domain scal  
vtp password 1234  
interface range f0/1 - 2  
switchport access vlan 70  
interface range f0/3 - 4  
switchport access vlan 80  
interface range f0/5 - 6  
switchport mode trunk  
switchport trunk allowed vlan 50,60,70,80  
ex  
spanning-tree mode pvst  
spanning-tree vlan 50,60 root secondary   
spanning-tree vlan 70,80 root primary  
ex  
wr  
