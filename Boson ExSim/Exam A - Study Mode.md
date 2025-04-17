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