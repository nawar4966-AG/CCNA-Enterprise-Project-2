# Server0 Configuration (Manually)
## 1. IPv4 Configuration 
IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1

## 2. TFTP Service
TFTP Service: Enabled

## 3. SYSLOG Service
Syslog Service: Enabled

## 4. NTP Service
NTP Server: Enabled
Authentication: Enabled
Key ID: 123
Password: 123

## 5. Notes
- This server is used as:
    - TFTP backup server for running-configs for R1-4 and ISP.
    - NTP server for time sync for R1
    - Syslog collector for R1

