# OSPF Part 1
### Things We'll Cover
- Basic OSP Operations (Introduction)
- OSPF Areas
- Basic OSPF Configuration
### Link State Routing Protocols
- When using a **link state** routing protocol, every router creates a 'connectivity map' of the network
- To allow this, each router advertises information about its interfaces (connected networks) to its neighbors
- These advertisements are passed along to other routers, until all routers in the network develop the same map of the network
- Each router independently uses this map to calculate the best routes to each destination
- Link state protocols use more resources (CPU) on the router, because more information is shared
- However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols
### OSPF
- Stands for **Open Shortest Path First**
- Uses the **Shortest Path First** algorithm of Dutch computer scientist Edsger Dijkstra (aka **Dijkstra's Algorithm)
- Three versions:
	- OSPFv1 (1989): OLD, not in use anymore
	- OSPFv2 (1998): Used for IPv4
	- OSPFv3 (2008): Used for IPv6 (can also be used for IPv4, but usually v2 is used)
- Routers store information about the network in **LSAs (Link State Advertisements)**, which are organized in a structure called the **LSDB (Link State Database)**
- Routers will **flood** LSAs until all routers in the OSPF **area** develop the same map of the network (LSDB)
- In OSPF, there are three main steps in the process of sharing LSAs and determining the best route to each destination in the network:
	1. **Become neighbors** with other routers connected to the same segment
	2. **Exchange LSAs** with neighbor routers
	3. **Calculate the best routes** to each destination, and insert them into the routing table
### OSPF Areas
- OSPF uses **areas** to divide up the network
- Small networks can be single-area without any negative effects on performance
- In larger networks, a single-area design can have negative effects:
	- The SPF algorithm takes more time to calculate routes
	- It also requires exponentially more processing power on the routers
	- The larger LSDB takes up more memory on  the routers
	- Any small change in the network causes every router to flood LSAs and run the SPF algorithm again
- By dividing a large OSPF network into several smaller areas, you can avoid the above negative effects
- An **area** is a set of routers and links that share the same LSDB
- The **backbone area** (area 0) is an area that all other areas must connect to
- Routers with all interface in the same area are called **internal routers**
- Routers with interfaces in multiple areas are called **area border routers (ABRs)**
	- ABRs maintain a separate LSDB for each area they're connected to
	- It's recommended that you connect an ABR to a maximum of 2 areas
	- Connecting an ABR to 3+ areas can overburden the router
- Routers connected to the backbone area (area 0) are called **backbone routers**
- An **intra-area route** is a route to a destination inside the same OSPF area
- An **interarea route** is a route to a destination in a different OSPF area
- OSPF areas should be *contiguous*
- All OSPF areas must have at least one ABR connected to the backbone area
- OSPF interfaces in the same subnet must be in the same area
### Basic OSPF Configurations
- To enter OSPF config mode, use the command `router ospf [process-id]`
	- The OSPF *process ID* is **locally significant**
	- Routers with different process IDs can become OSPF neighbors
- Next, configure the network using the command `network [ip-address] [wildcard-mask] area [area-id]`
	- The OSPF `network` command requires you to specify the **area**
	- For the CCNA, you only need to configure single-area OSPF (area 0)
- The `network` command tells OSPF to:
	- Look for any interfaces with an IP address contained in the range specified in the `network` command
	- Activate OSPF on the interface in the specified **area**
	- The router will then try to become OSPF neighbors with other OSPF-activated neighbor routers
### The `passive-interface` Command
- `passive-interface [interface-id]`
- This command tells the router to stop sending OSPF 'hello' messages out of the interface
- However, the router will continue to send LSAs informing its neighbors about the subnet configured on the interface
- You should always use this command on interfaces which don't have any OSPF neighbors
### Advertise a Default Route into OSPF
- First, configure the default route using `ip route 0.0.0.0 0.0.0.0 [next-hop]`
- To advertise this default route into OSPF is `default-information originate`
### `show ip protocols`
- In OSPF, to manually configure a router-id you can use the command `router-id [A.B.C.D]`
- To reflect these changes, use the command `clear ip ospf process`
	- This is a bad idea in a real network, and a router will lose all of its OSPF routes for a short time and won't be able to forward traffic to those destinations
- An **autonomous system boundary router** (ASBR) is an OSPF router that connects the OSPF network to an external network
- R1 is connected to the internet. By using the **default-information originate** command, R1 becomes an ASBR