en  
conf t  
hostname SW3  
vtp mode server  
vtp domain scal  
vtp password 1234  
vlan 90  
name i  
vlan 100  
name j  
vlan 110  
name k  
vlan 120  
name l  
ex  
interface range f0/1 - 9  
switchport mode trunk  
interface range f0/1 - 4  
channel-group 1 mode active  
ex  
interface range f0/5 - 8  
channel-group 2 mode active  
end  
wr  
