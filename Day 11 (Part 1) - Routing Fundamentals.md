# Routing Fundamentals
### Things We'll Cover
- What is routing?
- The routing table on a Cisco router
	- **Connected** and **Local** routes
- Routing fundamentals (route selection)
### What is Routing?
- **Routing** is the process that routers use to determine the path that IP packets shoud take over a network to reach their destination
	- Routers store routes to all of their known destinations in a **routing table**
	- When routers receive packets, they look in the **routing table** to find the best route to forward that packet
- There are two main routing methods (methods that routers use to learn routes):
- **Dynamic Routing**: Routers use *dynamic routing protocols* (i.e. OSPF) to share routing information with each other automatically and build their routing tables
	- This will be covered later in the course
- **Static Routing**: A network engineer/admin manually configures routes on the router
	- This will be covered in the next lecture
- A **route** tells the router: *to send a packet to destination X, you should send the packet to **next-hop** Y*
	- **next-hop** = the next router in the path to the destination
	- or, if the destination is directly connected to the router, send the packet directly to the destination
	- or, if the destination is the router's own IP address, receive the packet for yourself (don't forward it)
- ![](attachments/Pasted%20image%2020240924131801.png)
- There are four routers connected together, and they represent a **WAN**
- **WAN (Wide Area Network)** = a network that extends over a large geographical area
- For example, each of these four routers could be in different cities or even different countries
- Connected to R1 and R4, there are two **LANs**
- In the next lecture, we will configure **static routes** on the routers to allow PC1 and PC4 to communicate with each other
	- This lecture will focus on two types of routes automatically added to a router's routing table
### R1 Pre-Configurations (IP Addresses)
![](attachments/Pasted%20image%2020240924132024.png)
### Routing Table (`show ip route`)
![](attachments/Pasted%20image%2020240924132210.png)
### Connected and Local Routes
![](attachments/Pasted%20image%2020240924132354.png)
- A **connected** route is a route to the network
- R1 G0/2 IP = 192.168.1.1/24
- Network Address = 192.168.1.0/24
- It provides a route to all hosts in that network (i.e. 192.168.1.10, .100, .232, etc.)
- R1 knows: "If I need to send a packet to any host in the 192.168.1.0/24 network, I should send it out of G0/2"