# First Hop Redundancy Protocols
### Things We'll Cover
- The purpose of FHRPs
- **HSRP** (Hot Standby Router Protocol)
- **VRRP** (Virtual Router Redundancy Protocol)
- **GLBP** (Gateway Load Balancing Protocol)
- Basic HSRP configurations
### First Hop Redundancy Protocols
- A FHRP is a computer networking protocol which is designed to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address
- In the event of failure of an active router, the backup router will take over the address. usually within a few seconds
- A **virtual IP** is configured on the two routers, and a **virtual MAC** is generated for the virtual IP
	- Each FHRP uses a different format for the virtual MAC
- End hosts in the network are configured to use the virtual IP as their default gateway
- The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it
- If the active router fails, the standby becomes the next active router
- The new active router will send **gratuitous ARP** messages so that switches will update their MAC address tables
	- **Gratuitous ARP**: ARP replies sent wihout being requested (no ARP request message was received)
	- The frames are broadcast to FFFF.FFFF.FFFF (normal ARIP replies are unicast)
- Once this process is done, the new active router should now function as the default gateway
- FHRPs are **non-preemptive**
	- The current active router will not automatically give up its role, even if the former active router returns
	- You can change this setting to make R1 'preempt' R2 and take back its active role automatically
### HSRP (Hot Standby Router Protocol)
- Cisco proprietary
- An **active** and **standby** router are elected
- There are two versions: **version 1** and **version 2**
	- Version 2 adds IPv6 support and increases the number of *groups* that can be configured
- Multicast IPv4 address:
	- v1 = `224.0.0.2`
	- v2 = `224.0.0.102`
- Virtual MAC addresses (**IMPORTANT TO REMEMBER FOR THE EXAM**):
	- v1 = `0000.0c07.acXXX` (**XX** = HSRP group number)
	- v2 = `0000.0c9f.fXXX` (**XXX** = HSRP group number)
- In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet/VLAN to load balance- If the old active router comes back online, by default it won't take back its role as the active router
- It'll become the standby router
### VRRP (Virtual Router Redundancy Protocol)
- Open standard
- Almost identical to HSRP
- A **master** and **backup** router are elected (instead of active and standby)
- Multicast IPv4 address: `224.0.18`
- Virtual MAC address: `0000.5e00.01XX` (**XX** = VRRP group number)
- In a situation with multiple subnets/VLANs, you can configure a different master router in each subnet/VLAN to load balance
### GLBP (Gateway Load Balancing Protocol)
- Cisco proprietary
- Load balances among multiple routers *within a single subnet*
- An **AVG (Active Virtual Gateway) is elected
- Up to four **AVFs (Active Virtual Forwarders)** are assigned by the AVG (the AVG itself can be an AVF too)
- Each AVF acts as the default gateway for a portion of the hosts in the subnet
- Multicast IPv4 address: `224.0.0.102`
- Virtual MAC address: `0007.b400.XXYY`
	- **XX** = GLBP group number
	- **YY** = AVF number
### Comparing FHRPs
![](attachments/Pasted%20image%2020250223184428.png)
- Remembering the basic characteristics of all three and memorizing the info on this chart be enough to answer FHRP questions on the exam
### Configuring HSRP
 - To configure a v1 HSRP and specify its group number, we'd use the following command: `R1(config-if)#standby <0-255>`
 - To change it to version 2, use `standby version 2`
	 - This gives access to group numbers 0-4095
- The HSRP group number doesn't have to match the VLAN number for it to work, but it's generally a good idea to set it that way
	- This number **does have to match** between both routers for it to work
- To set the virtual IP, use the command `standby [group-number] ip [address]`
- To set the priority, use `standby [group-number] priority <0-255>`
	- The **active router** in this order:
		1. Highest priority (default 100)
		2. Highest IP address
- Finally, enable pre-emption using `standby [group-number] preempt`
	- **Preempt** causes the router to take the role of active router, even if another router already has the role
	- This is only necessary to enable on the router you want to become active
- When doing the same configuration, make sure to use the same HSRP version number
	- Versions 1 and 2 aren't compatible
	- If R1 uses v2, R2 must use the same version
- The virtual IP should be the same
**Summary:**
- `standby version 2`
- `standby [group-number] ip [virtual-ip]`
- `standby [group-number] priority [priority]`
- `standby [group-number] preempt`