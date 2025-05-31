SWITCH Configuration
----------
enable
configure terminal
hostname LAN

! Enable DHCP Snooping globally
ip dhcp snooping

! Enable DHCP Snooping only for VLAN 16
ip dhcp snooping vlan 16

! Disable Option 82 insertion
no ip dhcp snooping information option

! Interface Configurations
interface FastEthernet0/1
 description to_PC1
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/2
 description to_PC2
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast
 spanning-tree bpduguard enable

interface FastEthernet0/3
 description to_DHCP_Server
 switchport mode access
 switchport access vlan 16
 ip dhcp snooping trust
 spanning-tree portfast
 spanning-tree bpduguard enable

interface GigabitEthernet0/1
 description to_R1
 switchport mode trunk
 switchport trunk allowed vlan 16



Router R1 ----------------

enable
configure terminal
hostname R1

! VLAN 16 Subinterface
interface GigabitEthernet0/0.16
 description to_LAN
 encapsulation dot1Q 16
 ip address 172.16.16.1 255.255.255.240

! Point-to-point link to R2
interface GigabitEthernet0/1
 description to_R2
 ip address 10.10.12.1 255.255.255.252
 duplex auto
 speed auto

! Static route to reach R2's Loopback
ip route 2.2.2.2 255.255.255.255 10.10.12.2



Router R2 ----------


enable
configure terminal
hostname R2

! Enable password encryption
service password-encryption

! Enable password (unencrypted here, for Packet Tracer demo)
enable password admin

! Local user for SSH
username admin password admin

! Set domain name for SSH
ip domain-name test.lab

! Generate RSA key for SSH
crypto key generate rsa
! Choose 1024 bits when prompted

! Enable SSH v2
ip ssh version 2

! VTY line configuration
line vty 0 15
 login local
 transport input ssh

! Loopback interface
interface Loopback0
 ip address 2.2.2.2 255.255.255.255

! Interface to R1
interface GigabitEthernet0/1
 description to_R1
 ip address 10.10.12.2 255.255.255.252
 duplex auto
 speed auto

! Static route to VLAN 16 network
ip route 172.16.16.0 255.255.255.240 10.10.12.1

----------------------------------
DHCP
🖥️ DHCP Server (in Packet Tracer)
Interface Settings
Interface: FastEthernet0

IP Address: 172.16.16.2

Subnet Mask: 255.255.255.240

Default Gateway: 172.16.16.1

DHCP Configuration
Go to Services tab > DHCP

Configure:

Service: On

Pool Name: VLAN16_POOL

Default Gateway: 172.16.16.1

DNS Server: 172.16.16.2

Start IP Address: 172.16.16.3

Subnet Mask: 255.255.255.240

Maximum Number of Users: 10

Click Add
