# Verification Guide and Screenshots
This document contains all validation commands required to verify the functionality of the entire topology.
Each section includes the commands to run and the expected results.

## 1. Layer 3 Routing Verification
### 1.1 OSPF Neighbor Adjacency (R1-4&ISP)
Run on: R1, R2, R3, R4, ISP
`show ip ospf neighbor`
Expected:
- At least one FULL adjacency per router (usually with ISP).
  ![[Pasted image 20251130185151.png]]
### 1.2 Routing Table Verification
Run on all routers:
`show ip route`
Expected:
- OSPF routes (marked with “O”) for all remote VLAN subnets.
- Connected gateways for local VLANs (marked “C”).
- Serial WAN links marked “C” on each side.
  ![[Pasted image 20251130185714.png]]

## 2. PPP & CHAP Authentication
Run on each router’s serial interface:
`show interfaces s0/1/0`
Expected:
- Line protocol: up
- Encapsulation: PPP
  ![[Pasted image 20251130190018.png]]
Check CHAP:
`show run`
Expected:
- CHAP authenticated
  ![[Pasted image 20251130190328.png]]

## 3. DHCP (R1-4)
### 3.1 DHCP Bindings
Run on each router:
`show ip dhcp binding`
Expected:
- Dynamic bindings for PCs in each VLAN
  ![[Pasted image 20251130192851.png]]
### 3.2 DHCP Pools
`show ip dhcp pool`
Expected:
- Pools for VLANs with correct usage counters.
  ![[Pasted image 20251130193302.png]]

## 4. VLAN, VTP & Trunking Verification
### 4.1 VLAN Existence
Run on any switch:
`show vlan brief`
Expected:
- VLAN IDs 10–160 depending on site
- Ports in correct VLANs
- Server0 should appear in VLAN10
### 4.2 VTP Status
Run on switches:
`show vtp status`
Expected:
- VTP domain matches site (scal, scal2, scal3, scal4)
- Server switches: Mode = server
- Downstream switches: Mode = client
### 4.3 Trunk Verification
`show interfaces trunk`
Expected:
- Correct allowed VLANs
- EtherChannel interfaces bundled as Po1 / Po2
- Mode: trunk / LACP trunk

## 5. EtherChannel Verification
On SW1, SW1.1, SW1.2, SW3, SW3.1, SW3.2:
`show etherchannel summary`
Expected:
- LACP protocols
- Ports marked (P) for “bundled and in use”


## 6. Spanning Tree (PVST)
### 6.1 STP Mode
Run on distribution/access switches:
`show spanning-tree summary`
Expected:
- Mode: pvst
- No topology changes after convergence
### 6.2 Per VLAN Root Role
Test a few VLANs depending on site:
`show spanning-tree vlan 10 show spanning-tree vlan 20 show spanning-tree vlan 50 show spanning-tree vlan 70`
Expected:
- Primary root matches design (SW2.1 for VLAN 50/60 etc.)
- Secondary root matches opposite distribution switch

## 7. Management Plane Verification
### 7.1 Syslog (Server0)
On Server0, verify Syslog window receives logs.
Generate a log message on R1:
`clear logging logging host 192.168.10.10 logging trap debugging send log TEST-SYSLOG-MESSAGE`
Expected:
- Message appears on Server0 Syslog panel.
### 7.2 NTP Synchronization
Run on each router:
`show clock show ntp associations`
Expected:
- Clock synchronized
- Server reachable (192.168.10.10)
### 7.3 TFTP Backup
Run on each router:
`copy run tftp: Address or name of remote host []? 192.168.10.10 Destination filename? R1_backup`
Expected:
- “Write file OK”
- File appears in TFTP directory on Server0

## 8. ACL Behavior Verification
Test each ACL from the affected PCs:
### ACL 1: VLAN 10 cannot ping VLAN 70
From a PC in VLAN 10:
`ping 192.168.70.x`
Expected: Fail
### ACL 2: VLAN 80 cannot ping VLAN 130/140
From a PC in VLAN 80:
`ping 192.168.130.x ping 192.168.140.x`
Expected: Fail
### ACL 3: PC23 cannot HTTP to Server0
From PC23 (VLAN 120):
`http://192.168.10.10`
Expected: Blocked
### ACL 4: PC30 cannot ping Server0
From PC30 (VLAN 160):
`ping 192.168.10.10`
Expected: Fail
All other legitimate flows should succeed.


## 9. End-to-End Connectivity Tests
From various PCs:
`ping 192.168.10.10 ping 1.1.1.1         (ISP loopback) traceroute 192.168.10.10`
Expected:
- All permitted traffic reaches the destination
- OSPF-learned routes used in traceroute


## 10. Final Status Check (All Devices)
Run:
`show ip interface brief show running-config show startup-config write memory`
Expected:
- No down/down critical interfaces
- Configs saved properly
- No unexpected VLANs or trunks