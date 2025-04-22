### Question 1
![](attachments/6744de848004d95d05920bac28ad6df4.png)
- The answer is A because standard ACLs have a number range of 1-99, or 1300-1999 as stated in [day 34](../Day%2034%20-%20Standard%20ACLs.md#^979b85), wheras extended ACLs have a range of 100-199 or 2000-2699
### Question 2
![](attachments/8e2892ce5f3996ef7c6bb0d75f60d8bd.png)
- D is the correct answer because the `ip arp inspection trust` command was never issued
- By default, a port becomes untrusted when DAI is enabled
### Question 4
![](attachments/28327043939d47fce15d197c82c86a05.png)
- C is correct: RouterA forwards DHCP packets for NetworkA devices
- `ip helper-address` should point to the DHCP server’s IP (not the router, unless it's also the DHCP server)
- Multiple `ip helper-address` commands can be used for multiple DHCP servers
	- This also applies to networks using separate servers for DHCP, DNS, and TFTP
- Command should be configured on Fa0/0 to forward UDP broadcasts to NetworkB
### Question 5
![](attachments/48be5cf8b657313f299f11786e961fd6.png)
- A is correct: In **transport mode**, IPSec encrypts only the IP packet's payload
- The original IP header remains unchanged
- **Tunnel mode** encrypts both header and payload
- Transport mode isn’t required for NAT traversal and is less NAT-compatible than tunnel mode
- Tunnel mode adds extra headers due to full packet encryption
### Question 6
![](attachments/17e6ff6781272eac563eb55f2a114f8f.png)
- The CAM table is AKA the MAC address table, which records source MAC addresses
### Question 9
![](attachments/b33178a534e10ef7cd1a9b5cfcebeaed.png)
- HostA's subnet mask is /27 (27 network bits, 5 host bits)
- Subnet allows 2⁵ = 32 addresses
- Network divided into eight /27 subnets:
    - 10.10.2.0/27
    - 10.10.2.32/27
    - 10.10.2.64/27
    - 10.10.2.96/27
    - 10.10.2.128/27
    - 10.10.2.160/27
    - 10.10.2.192/27
    - 10.10.2.224/27
- HostA (10.10.2.101) is in the 10.10.2.96/27 subnet
- Default gateway (10.10.2.1) is outside HostA's subnet
- Using a broader subnet mask (e.g., /24 or /25) would enable HostA to reach devices beyond RouterA, including internet access
### Question 11
![](attachments/e8ee156daabdef1129f91108532e2452.png)
- A is correct: Layer 3 devices block broadcast traffic, creating separate broadcast domains
- This helps reduce broadcast storms, especially in large Layer 2 WANs during instability
- Adding more Layer 2 devices expands the broadcast domain, increasing the risk of broadcast storms
### Question 12
![](attachments/2961af3d7458ee1d27bac568fcaddb7f.png)
- C is correct: **Cisco DNA Center** supports Cisco Software-Defined Access (SDA)
- SDA builds LANs using policies and automation instead of traditional methods
- DNA Center uses a centralized controller and GUI to simplify network configuration
- Cisco IOS: CLI-based device configuration and management
- Cisco Network Assistant: Older desktop GUI tool for LAN management (pre-SDA)
- Cisco Prime Infrastructure: Browser-based GUI for traditional enterprise network management
### Question 13
![](attachments/f026a0d89bf7cfc74803af3592d2985e.png)
- **B & D are correct**: Broadcast and point-to-point network types use a 10s hello timer and 40s dead timer by default
- Nonbroadcast, point-to-multipoint, and point-to-multipoint nonbroadcast types use a 30s hello timer and 120s dead timer
### Question 14
![](attachments/3714bdce6b7729104503c017187e8f72.png)
- **B is correct**: The virtual interface on a WLC supports mobility management for wireless clients
- A WLC can have up to four static interfaces:
    - **Management interface**: Used for in-band management
    - **AP-manager interface**: Contains the source IP address used by lightweight APs to communicate with the WLC
    - **Virtual interface**: Supports mobility by using a consistent IP address across multiple controllers when clients roam
    - **Service port interface**: Used for out-of-band maintenance
- **Dynamic interfaces**: User-defined, typically used for wireless client data; function similarly to VLANs
### Question 16
![](attachments/59e7ad457bfc716974d531fd04354403.png)
- **A is correct:** The `ntp master` command makes a Cisco router act as an NTP server using its software clock as the authoritative time source
- If NTP wasn't already enabled, this command also allows the router to respond to NTP queries on all active interfaces
- When an NTP device uses its software clock as the time source, it assigns the IP address **127.127.7.1** as both the reference clock and the NTP server address
- By default, a Cisco device using NTP syncs only its **software clock**, not the **hardware clock**
### Question 17
![](attachments/3a323fa5692693289afba4cbb8a88cbb.png)
- **B is correct:** The **Frame Control (FC)** field in an 802.11 MAC frame indicates if the frame is a management frame
- The **Duration (DUR)** field is mainly used by control frames to indicate transmission timers
- The **Sequence (SEQ)** field is split into two parts: **fragment number** and **sequence number** of the frame
### Question 18
![](attachments/fe6ace8548ad11c6fcb22ced2079ada9.png)
- **C is correct:** WPA3-Personal uses **Simultaneous Authentication of Equals (SAE)** and supports **Protected Management Frames (PMF)**
- **PMF** secures management frame communication between the access point and the client
- **SAE** allows mutual authentication between clients and access points
### Question 19
![](attachments/49ab364cf9183c7cb7fe40e180b764e2.png)
- **A & C are correct:** The `switchport access vlan 4` command moves the port to an unused VLAN
	- "No devices operate on VLAN 4"
- The `switchport mode access` command manually sets the port mode
- Neither command disables DTP
- By default, Cisco switch interfaces use DTP to auto-negotiate trunk or access mode
- Use `switchport nonegotiate` on manually configured ports to stop DTP negotiation attempts
- Manually setting trunk or access mode limits DTP, but `switchport nonegotiate` is still recommended
- Manually configured trunk ports will still send DTP frames
### Question 23
![](attachments/1e93ef4bcda5d70f5b977025324ccbc2.png)
- **D is correct:** The network prefix identifies the network subnet that is reachable through the particular routing table entry
- The network prefix can be paired with a network mask, which represents the subnet mask for the associated subnet
- The four octets in the `200.120.45.192` prefix represent the network address of the subnet, which is `200.120.45.192`
- Therefore, the output in the routing table is the network address for the `200.120.45.192/28` network
- The `/28` CIDR notation represents the subnet mask of the network, which is `255.255.255.240`
- In binary:  
    `255.255.255.240` = `11111111.11111111.11111111.11110000`
- With 4 host bits, you get **2⁴ = 16** total IP addresses in this subnet
- Start at the **network address**: `200.120.45.192`
- Add 15 to get the **broadcast address**: `200.120.45.192 + 15 = 200.120.45.207`
    - The first address is reserved for the **network**, and the last is for the **broadcast**
- Therefore, the broadcast address for this network is `200.120.45.207` because it's the last IP in the 16-address block
### Question 24
![](attachments/935d1c8c5be31d2896b9210039ad46c7.png)
- **B is correct:** The output shows that Router1 is in the `DROTHER` state
- A router in the `DROTHER` state can only form adjacencies with the **Designated Router (DR)** and **Backup Designated Router (BDR)**
- Router1 isn’t on a point-to-multipoint network, as the segment has a DR and BDR
- DR and BDR elections occur only on **multiaccess networks**, not on point-to-multipoint or point-to-point networks
- The BDR's priority might be above or below 50
- If Router1 starts after the DR and BDR, it can't become DR or BDR until the current ones fail or are powered off
- If Router1 starts simultaneously with the DR and BDR, the BDR must have a priority of at least 50
- If the BDR and Router1 share the same priority, the BDR wins due to a higher router ID (10.0.0.11 vs 10.0.0.4)
- Incorrect timer settings on Router1 would prevent it from establishing adjacencies with the DR and BDR
### Question 25
![](attachments/1fa9deed24da86bd638615e8e66fea26.png)
- **D is correct:** ACL 10 is applied as an **outbound ACL** on interface F0/1, so it's processed for outbound traffic
- Outbound ACLs are checked **after** the router receives and routes the packet to the outbound interface
- RouterA does **not** check ACL 10 when the packet enters F0/0, since ACL 10 isn't applied inbound on that interface
- Inbound ACLs are processed **after receiving** a packet but **before routing** it to the outbound interface
### Question 29
![](attachments/9c4cd9c72f7e0c919452e126ab984404.png)
- **E is correct**
- In this scenario, the MAC address 00-33-66-99-BB-EE has an OUI of 00-33-66 and a NIC idetnfier of 99-BB-EE
- To create an EUI-64 interface ID, you split the MAC address in half, add FFFE to the middle, and invert the 7th (U/L) bit
- The hex value 00 in the first 8 bits of the OUI can be represented in binary as 0000 0000
- After inverting, this results in 0000 00**1**0
- When converted back to hex, this becomes 02
- That means the EUI-64 interface ID is now **02**33:66FF:FE99:BBEE
- This is then combined with the IPv6 prefix to create an IPv6 address
- Appending the prefix 2012::99 to this interface ID creates the global unicast IPv6 address of 2012::99:0233:66FF:FE99:BBEE
### Question 32
![](attachments/67b61ac944084e0cc29aa29b4adb482f.png)
- **D is correct**
- The `ipv6 traffic-filter` command can be used in interface config mode to apply an ACL to an IPv6 interface
- The syntax for this command is `ipv6 traffic-filter (ipv6-acl) [in | out]`
- The `access-class` command can be used to apply an IPv4 ACL to a vty, AUX, or console line
- For IPv6, the `ipv6 access-class` command can be used
### Question 33
![](attachments/9a85c4b365f435878a673275ff978d8a.png)
- **Correct answers: C & D**
- `default-information originate` injects a router's default external route into OSPF
- Issuing `default-information originate` or `redistribute (network)` makes the router an ASBR
- ASBR redistributes routes from other routing protocols into OSPF
- Using the command on RouterA makes it RouterB's gateway of last resort
- RouterB becomes RouterC's gateway of last resort via RouterA
- This command does **NOT** advertise all directly connected routes
- Use `redistribute connected` to advertise directly connected routes
- `summary-address (address mask)` summarizes redistributed routes in OSPF
### Question 36
![](attachments/14e65b1ff75db9a49c367f979f6f99b3.png)
- **B is correct**
- `switchport voice vlan` has four options:
	- *vlan-id*
	- `dot1p`
	- `untagged`
	- `none`
- `dot1p` sends voice traffic with 802.1p priority 5 using VLAN 0; requires VLAN ID but not a unique voice VLAN
- This can be seen below:
![](attachments/5e8c27273035d3ac5079af198414ec38.png)
- `none` is the default mode; IP phone sends untagged voice and data traffic over the access VLAN
![](attachments/d58662dc38bd853699b30fe919989813.png)
- `switchport voice vlan 10` sends voice traffic over VLAN 10 (a unique VLAN)
![](attachments/9f7e2f849c5a5a4cbe0f3933b9ee529b.png)
- `untagged` sends both voice and data as untagged over the native VLAN
- Both voice and data traffic use a special-case 802.1Q trunk:
![](attachments/f6be3bcd540f5449b0611d859e71b123.png)
### Question 38
![](attachments/ce282cea738a35e4af1b3d010f9cd731.png)
- **Correct answers: B & C**
- Default `switchport port-security` settings (if not previously configured):
    - Sticky MAC learning disabled
    - Max 1 MAC address allowed
    - Violation mode set to _Shutdown_
    - Aging time set to 0
    - Static aging disabled
    - Aging type set to _absolute_
- F0/1 allows only 1 MAC address and uses `protect` mode
- Only the first host can access the network
- Traffic from the second host is discarded; port continues working for the first
- Port security supports both static and dynamically-learned MAC addresses by default
### Question 39
![](attachments/1da178a0c12f86c650ffc50dedd00269.png)
- **A is correct**
- EIGRP supports load balancing over equal-cost and unequal-cost paths
- OSPF only supports equal-cost load balancing