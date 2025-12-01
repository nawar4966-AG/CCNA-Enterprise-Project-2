# Verification Guide and Screenshots
This document contains all validation commands required to verify the functionality of the entire topology.
Each section includes the commands to run and the expected results.

## 1. Layer 3 Routing Verification
### 1.1 OSPF Neighbor Adjacency (R1-4&ISP)
Run on: R1, R2, R3, R4, ISP
`show ip ospf neighbor`
Expected:
- At least one FULL adjacency per router (usually with ISP).

  <img width="571" height="99" alt="Pasted image 20251130185151" src="https://github.com/user-attachments/assets/a390d34c-51aa-4da6-b1cf-7ffce965d58f" />

### 1.2 Routing Table Verification
Run on all routers:
`show ip route`
Expected:
- OSPF routes (marked with “O”) for all remote VLAN subnets.
- Connected gateways for local VLANs (marked “C”).
- Serial WAN links marked “C” on each side.

  <img width="597" height="629" alt="image" src="https://github.com/user-attachments/assets/cbba1711-10ae-4be4-9f91-be2af1d4f451" />



## 2. PPP & CHAP Authentication
Run on each router’s serial interface:
`show interfaces s0/1/0`
Expected:
- Line protocol: up
- Encapsulation: PPP

  <img width="548" height="421" alt="Pasted image 20251130190018" src="https://github.com/user-attachments/assets/bf5f41f6-2cdf-4e16-a55b-805bce24f9f9" />

Check CHAP:
`show run`
Expected:
- CHAP authenticated

  <img width="334" height="78" alt="Pasted image 20251130190328" src="https://github.com/user-attachments/assets/c114ad03-2cae-432e-92e9-7c5e406630fd" />


## 3. DHCP (R1-4)
### 3.1 DHCP Bindings
Run on each router:
`show ip dhcp binding`
Expected:
- Dynamic bindings for PCs in each VLAN

  <img width="566" height="178" alt="Pasted image 20251130192851" src="https://github.com/user-attachments/assets/7abe0e14-0075-493d-a411-1e28e8a9e68d" />

### 3.2 DHCP Pools
`show ip dhcp pool`
Expected:
- Pools for VLANs with correct usage counters.

  <img width="591" height="662" alt="Pasted image 20251130193302" src="https://github.com/user-attachments/assets/00017467-62a1-4263-835d-b0c365a66162" />


## 4. VLAN, VTP & Trunking Verification
### 4.1 VLAN Existence
Run on any switch:
`show vlan brief`
Expected:
- VLAN IDs 10–160 depending on site
- Ports in correct VLANs
- Server0 should appear in VLAN10

<img width="564" height="240" alt="image" src="https://github.com/user-attachments/assets/5babfc39-3fe8-437e-afdc-58e1737b9d1e" />
<img width="564" height="252" alt="image" src="https://github.com/user-attachments/assets/41aaf165-dfb2-44b0-a7e0-6dfebdc1ee97" />


### 4.2 VTP Status
Run on switches:
`show vtp status`
Expected:
- VTP domain matches site (scal, scal2, scal3, scal4)
- Server switches: Mode = server
- Downstream switches: Mode = client

<img width="531" height="190" alt="image" src="https://github.com/user-attachments/assets/98b824df-a5b1-4b87-bd3d-5e80cdb558e6" />

<img width="544" height="240" alt="image" src="https://github.com/user-attachments/assets/f7dad601-9527-44bf-bc20-f682411db7ae" />



### 4.3 Trunk Verification
`show interfaces trunk`
Expected:
- Correct allowed VLANs
- EtherChannel interfaces bundled as Po1 and Po2
- Mode: trunk

<img width="501" height="294" alt="image" src="https://github.com/user-attachments/assets/2e9b9ef2-e707-480f-904a-28b471f16a6c" />

## 5. EtherChannel Verification
On SW1, SW1.1, SW1.2, SW3, SW3.1, SW3.2:
`show etherchannel summary`
Expected:
- LACP protocols
- Ports marked (P) for “bundled and in use”

<img width="497" height="270" alt="image" src="https://github.com/user-attachments/assets/3860bd16-09ba-4a8d-8528-fd2e25076178" />


## 6. Spanning Tree (PVST)
### 6.1 STP Mode
Run on distribution/access switches:
`show spanning-tree summary`
Expected:
- Mode: pvst
- Each assigned VLAN is blocking one port

<img width="521" height="336" alt="image" src="https://github.com/user-attachments/assets/087d345d-6058-4420-9c87-8ed9d9fa8dc8" />


### 6.2 Per VLAN Root Role
On SW2, SW2.1, SW2.2, SW4, SW4.1, SW4.2:
`show spanning-tree`
Expected:
- Primary root matches design (SW2.1 for VLAN 50/60 etc.)
- Secondary root matches opposite distribution switch

<img width="617" height="756" alt="image" src="https://github.com/user-attachments/assets/498150d2-8a5d-4a7c-968a-7436b0f4a0b7" />
<img width="648" height="671" alt="image" src="https://github.com/user-attachments/assets/47c93683-0a8f-48df-998c-096c19136bfe" />

## 7. Management Plane Verification
### 7.1 Syslog (Server0)
On Server0, verify Syslog window receives logs.
Generate a log message on R1:
`en`
`conf t`
`logging host 192.168.10.10`  
`logging trap debugging`
`interface gig0/0/0`
`shutdown`
`no shutdown`
Expected:
- Message appears on Server0 Syslog panel.

<img width="1179" height="443" alt="image" src="https://github.com/user-attachments/assets/062fd2cc-fa47-44e5-9df9-30fc7233639a" />

### 7.2 NTP Synchronization
Run on each router:
`show clock`
`show ntp associations`
Expected:
- Clock synchronized
- Server reachable (192.168.10.10)

<img width="346" height="94" alt="image" src="https://github.com/user-attachments/assets/4642caca-fe80-4bb5-bad7-2f0d2d07ac7d" />
<img width="691" height="128" alt="image" src="https://github.com/user-attachments/assets/594b5dee-1296-49a6-b126-861014d49dc5" />

### 7.3 TFTP Backup
Run on each router:
`copy run tftp`
Address or name of remote host []? `192.168.10.10` 
Destination filename? `R1_backup`
Expected:
- “Write file OK”
- File appears in TFTP directory on Server0

<img width="754" height="458" alt="image" src="https://github.com/user-attachments/assets/f8adcc9e-ac2b-4c54-bb9d-c539c9b9eb0c" />


## 8. ACL Behavior Verification
Test each ACL from the affected PCs:
### ACL 1: VLAN 10 cannot ping VLAN 70
From a PC in VLAN 10:
`ping 192.168.70.2`
Expected: Fail once the ACL is applied on R1

<img width="573" height="531" alt="image" src="https://github.com/user-attachments/assets/034132c4-0167-4ae1-bffc-76dcbec9b5dc" />

### ACL 2: VLAN 80 cannot ping VLAN 130/140
From a PC in VLAN 80:
`ping 192.168.130.2` 
`ping 192.168.140.2`
Expected: Fail

<img width="1081" height="378" alt="image" src="https://github.com/user-attachments/assets/d1b21ea9-c22b-47a5-bc10-256bce763b8e" />

### ACL 3: PC23 cannot HTTP to Server0
From PC23 (VLAN 120):
`http://192.168.10.10`
Expected: Blocked

<img width="1021" height="466" alt="image" src="https://github.com/user-attachments/assets/2b73267e-494d-4af4-9080-51c0150ff5bc" />


### ACL 4: PC30 cannot ping Server0
From PC30 (VLAN 160):
`ping 192.168.10.10`
Expected: Fail

<img width="872" height="352" alt="image" src="https://github.com/user-attachments/assets/a5f22296-1052-462e-a6bc-c697cd084534" />


## 9. End-to-End Connectivity Tests
From various PCs:
`ping 192.168.10.10`

<img width="933" height="417" alt="image" src="https://github.com/user-attachments/assets/bd111ee5-6423-4022-a68a-4db6aca10aa8" />

From ISP:
`ping 1.1.1.1`
`traceroute 192.168.10.10`
Expected:
- All permitted traffic reaches the destination
- OSPF-learned routes used in traceroute passing on two router ports

<img width="510" height="120" alt="image" src="https://github.com/user-attachments/assets/ecbb431a-e91a-4323-a4b2-063e173673dc" />
<img width="366" height="120" alt="image" src="https://github.com/user-attachments/assets/6318a1c5-0b63-43a0-9d5b-15fa9718c6d5" />

## 10. Final Status Check (All Devices)
Run:
`show ip interface brief`
`show running-config`
`show startup-config`
Expected:
- No down/down critical interfaces
- Configs saved properly
- No unexpected VLANs or trunks
