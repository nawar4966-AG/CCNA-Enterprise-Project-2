# Multisite End-to-End Enterprise Network (CCNA-Level Project)
## Topology
<img width="1827" height="714" alt="image" src="https://github.com/user-attachments/assets/14b973ba-f427-48d8-9cb8-784de2409a8a" />


### Devices: 
- Routers: ISP, R1, R2, R3, R4
- Switches per site (supporting VLANs)
- End Devices (PCs)
- Server (in R1's LAN)

## Objective
A complete CCNA-level enterprise-style Layer 2 / Layer 3 lab demonstrating VLAN design with VTP, PVST, EtherChannel, router-on-a-stick inter-VLAN routing, OSPF over PPP (CHAP), DHCP services per site, ACL enforcement, and management-plane services (syslog, NTP, TFTP). 

## Key Features
### 1. Routing & WAN Protocols
- All routers (ISP, R1-R4) are in OSPF Area 0.
- Serial connections use PPP with CHAP authentication.
- Default routes are used to simplify routing.
### 2. VLANs & Spanning Tree
- VLANs configured (4 in each router area and 2 per each access switche): 10, 20, 30... up to 160.
- PVST used for the following VLAN pairs:
    - VLAN 50 & 60
    - VLAN 70 & 80
    - VLAN 130 & 140
    - VLAN 150 & 160
### 3. DHCP
- Each router assigns IPs for its LAN via DHCP.
- DHCP pools exclude default gateway addresses (and Server0 static IP address on R1).
### 4. EtherChannel
- EtherChannel Groups 1&2: Between Switches in R1’s network.
- EtherChannel Groups 1&2: Between Switches in R3’s network.
### 5. Extended Access Control Lists (ACLs)
- PC 23 Deny Server 0 HTTP
- PC 30 Deny Server 0 IP Ping
- VLAN 10 Deny Ping VLAN 70
- VLAN 80 Deny Ping VLAN 130/140
### 5. Server Functionality
- Located in R1’s LAN.
- Provides:
    - TFTP backup server for running-configs for R1-4 and ISP
    - NTP server for time sync for R1
    - Syslog collector for R1

## Project Goals
- Build a multi-site enterprise topology with proper L2 and L3 segmentation
- Implement VLANs and VTP for scalable VLAN distribution
- Use PVST to control per-VLAN root placement
- Implement EtherChannel for increased bandwidth and redundancy
- Configure inter-VLAN routing using router subinterfaces
- Establish PPP/CHAP links to simulate WAN connections
- Advertise all networks using OSPF Area 0
- Configure DHCP on routers for each VLAN
- Apply ACLs for realistic security policies
- Implement NTP, Syslog, and TFTP management services
- Validate full end-to-end connectivity and service functionality

## IP Static Configuration Table
### 1. Server0 (Located in R1 Network)
| Device  | Interface | IP Address    | Subnet          |
| ------- | --------- | ------------- | --------------- |
| Server0 | FA0       | 192.168.10.10 | 192.168.10.0/24 |

### 2. WAN Links (Router-to-Router via Serial) – /30 Subnets
| Link     | Subnet            | Device | Interface | IP Address     |
| -------- | ----------------- | ------ | --------- | -------------- |
| ISP - R1 | 192.168.200.0/30  | ISP    | S0/1/0    | 192.168.200.1  |
|          |                   | R1     | S0/1/0    | 192.168.200.2  |
| ISP - R2 | 192.168.200.4/30  | ISP    | S0/1/1    | 192.168.200.5  |
|          |                   | R2     | S0/1/0    | 192.168.200.6  |
| ISP - R3 | 192.168.200.8/30  | ISP    | S0/2/0    | 192.168.200.9  |
|          |                   | R3     | S0/1/0    | 192.168.200.10 |
| ISP - R4 | 192.168.200.12/30 | ISP    | S0/2/1    | 192.168.200.13 |
|          |                   | R4     | S0/1/0    | 192.168.200.14 |

### 3. VLAN Interfaces on Routers (Sub VLAN IFs for DHCP Gateway)
| Router | VLAN | Interface | Subnet           | IP Address    |
| ------ | ---- | --------- | ---------------- | ------------- |
| R1     | 10   | f0/0.10   | 192.168.10.0/24  | 192.168.10.1  |
| R1     | 20   | f0/0.20   | 192.168.20.0/24  | 192.168.20.1  |
| R1     | 30   | f0/0.30   | 192.168.30.0/24  | 192.168.30.1  |
| R1     | 40   | f0/0.40   | 192.168.40.0/24  | 192.168.40.1  |
| R2     | 50   | f0/0.50   | 192.168.50.0/24  | 192.168.50.1  |
| R2     | 60   | f0/0.60   | 192.168.60.0/24  | 192.168.60.1  |
| R2     | 70   | f0/0.70   | 192.168.70.0/24  | 192.168.70.1  |
| R2     | 80   | f0/0.80   | 192.168.80.0/24  | 192.168.80.1  |
| R3     | 90   | f0/0.90   | 192.168.90.0/24  | 192.168.90.1  |
| R3     | 100  | f0/0.100  | 192.168.100.0/24 | 192.168.100.1 |
| R3     | 110  | f0/0.110  | 192.168.110.0/24 | 192.168.110.1 |
| R3     | 120  | f0/0.120  | 192.168.120.0/24 | 192.168.120.1 |
| R4     | 130  | f0/0.130  | 192.168.130.0/24 | 192.168.130.1 |
| R4     | 140  | f0/0.140  | 192.168.140.0/24 | 192.168.140.1 |
| R4     | 150  | f0/0.150  | 192.168.150.0/24 | 192.168.150.1 |
| R4     | 160  | f0/0.160  | 192.168.160.0/24 | 192.168.160.1 |

### 4. Interface loopback on R1-4 Routers \32
| Device | Interface            | IP Address |
| ------ | -------------------- | ---------- |
| ISP    | interface Loopback 0 | 1.1.1.1    |
| R1     | interface Loopback 0 | 1.1.1.2    |
| R2     | interface Loopback 0 | 1.1.1.3    |
| R3     | interface Loopback 0 | 1.1.1.4    |
| R4     | interface Loopback 0 | 1.1.1.5    |

## Verification Checklist 
Essential Tests and expected results in Screenshots.
### Layer 3 Routing (R1-4&ISP)
- Verify OSPF routes:
	`show ip route`
- OSPF adjacency check:
	`show ip ospf neighbor`
- PPP/CHAP line protocol status:
	`show interfaces serial 0/1/0`
	`show run`
- Check reachability to Server0:
    `ping 192.168.10.10` 

### DHCP (R1-4)
- see the IPs assigned through DHCP on each PC and the corresponding MAC-Address:
	`show ip dhcp binding`
- PCs should receive DHCP addresses in their assigned VLANs.

### VLAN & Trunking (core & access Switches)
- Verify VLANs exist:
	`show vlan brief` 
- See trunking interfaces:
	`show interfaces trunk` 
- Confirm client/server mode:
	`show vtp status`

### EtherChannel (core & access Switches)
- Confirm channel-group status:
	`show etherchannel`
	`show etherchannel summary`

### PVST (access Switches)
- Make sure that Root primary/secondary placement correct per design (ex. VLAN 10&20 are primary on SW1.1).
	`show spanning-tree`
	`show spanning-tree summary`

### Management Services
- TFTP backups successful via checking the TFTP service on Server0:
	`copy run tftp`
- Clock synchronized via NTP:
    `show clock`
- Syslog messages visible on Server0

### ACL Behavior
- Test deny/permit scenarios from specific PCs:
    - PC30 → ping to Server0 blocked
      `ping 192.168.10.10`
    - PC23 → telnet to Server0 blocked
      `telnet 192.168.10.10`
    - VLAN 10 → ping to VLAN 70 blocked
      `ping 192.168.70.2`
    - VLAN 80 → ping to VLAN 130/140 blocked
      `ping 192.168.130.2`
      `ping 192.168.140.2`


## Notes:
- The Used version of Packet Tracer for this project is 9.0.0
- Before routers configurations it is necessary to add the Cisco Network Interface Module "NIM-2T" on each router (Router type 4321), except for ISP which needs two NIM-2T modules as each module provide two serial interfaces, steps are shown in the screenshots below
<img width="986" height="548" alt="image" src="https://github.com/user-attachments/assets/ea8c486f-f68b-4d84-ba59-137f128fe8c0" />
<img width="840" height="497" alt="image" src="https://github.com/user-attachments/assets/d461e7ee-06e7-4e03-9684-fe05ea9e1aed" />


- Make sure to start the configuration from Server0, the layer 2 perspective (core then access Switches) then moving on to the Layer 3 Routers (R1-4 then ISP), and at last testing DHCP, ACLs, Ping, Telnet on all PCs
