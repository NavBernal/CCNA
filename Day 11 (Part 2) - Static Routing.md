# Static Routing
### Things We'll Cover
- Review: **Connected** and **Local** routes
- Intro to Static Routes
- Static Route configuration
- Default Routes
### R2 Connected & Local Routes
![](attachments/039a248e63d4cd2aeb415df801e27ce7.png)
![](attachments/63cd29cd9355169697f46de09093ea7d.png)
### Routing Packets: Default Gateway
![](attachments/1e7759d7b3c34033d2b85167ee871f90.png)
- End hosts like PC1 and PC4 can send packets directly to destinations in their connected network
	- PC1 is connected to 192.168.1.0/24, PC4 is connected to 192.168.4.0/24
- To send packets to destinations outside of their local network, they must send the packets to their **default gateway**
![](attachments/804bc65ea3c11e2bc965723202796453.png)
- The **default gateway** configuration is also called a **default route**
	- It is a route to 0.0.0.0/0 = all netmask bits set to 0
	- Includes all addresses from 0.0.0.0 to 255.255.255.255
- The **default route** is the *least specific* route possible, because it includes all IP addresses
	- 0.0.0.0/0 = 4,294,IP addresses
- A /32 route (i.e. Local route) is the *most specific* route possible, because it specifies only one IP address
	- 192.168.1.1/32 = 1 IP address
- End hosts usually have no need for any more specific routes
	- They just need to know: to send packets outside of my local network, I should send them to my default gateway
![](attachments/ec7b6ed04b87524c6d6ade19ad943e99.png)
### Routing Packets: Static Routes
- When R1 receives the frame from PC1, it will de-encapsulate it (remove L2 header/trailer) and look at the inside packet
- It will check the routing table for the most-specific matching route:
![](attachments/10095b215eb2d822dd47d001265fd8fb.png)
- R1 has no matching routes in its routing table
	- It will drop the packet
- To properly forward the packet, R1 needs a route to the destination network (192.168.4.0/24)
	- Routers are instructions: *To send a packet to destinations in network 192.168.4.0/24, forward the packet to next hop Y*
- There are two possible path packets from PC1 to PC4 can take:
	- PC1 -> R1 -> R3 -> R4 -> PC4
	- PC1 -> R1 -> R2 -> R4 -> PC4
![](attachments/fc41abab1aaff76e9d352749ce5a6061.png)
- In this video, we will use the path via R3, not the path via R2
- It is possible to configure the routers to:
	- load-balance between path 1 and 2
	- Use path 1 as the main path and path 2 as a backup path
- These techniques will be talked about more later in the course