en
conf t
hostname ISP
username R1 password cisco
username R2 password cisco
username R3 password cisco
username R4 password cisco
interface Loopback 0
ip address 1.1.1.5 255.255.255.255  
ex
interface s0/1/0
ip address 192.168.200.1 255.255.255.252
encapsulation ppp
clock rate 64000
ppp authentication chap
no shutdown
ex
interface s0/1/1
ip address 192.168.200.5 255.255.255.252
encapsulation ppp
clock rate 64000
ppp authentication chap
no shutdown
ex
interface s0/2/0
ip address 192.168.200.9 255.255.255.252
encapsulation ppp
clock rate 64000
ppp authentication chap
no shutdown
ex
interface s0/2/1
ip address 192.168.200.13 255.255.255.252
encapsulation ppp
clock rate 64000
ppp authentication chap
no shutdown
ex
router ospf 1
network 192.168.200.0 0.0.0.3 area 0
network 192.168.200.4 0.0.0.3 area 0
network 192.168.200.8 0.0.0.3 area 0
network 192.168.200.12 0.0.0.3 area 0
network 1.1.1.5 0.0.0.0 area 0  
ex
ip route 0.0.0.0 0.0.0.0 s0/1/0  
ip route 0.0.0.0 0.0.0.0 s0/1/1
ip route 0.0.0.0 0.0.0.0 s0/2/0
ip route 0.0.0.0 0.0.0.0 s0/2/1
ex
wr
copy running-config tftp
192.168.10.10
ISP_backup
