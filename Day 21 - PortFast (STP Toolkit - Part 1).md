# PortFast (STP Toolkit - Part 1)
### Things We'll Cover
- STP Toolkit:
	- Portfast
		- Allows switch ports connected to end hosts to immediately enter the STP forwarding state, bypassing Listening and Learning
	- BPDU Guard
		- Automatically disables a port if it receives a BPDU, protecting the STP topology by preventing unauthorized devices from becoming part of the network
	- Root Guard
		- Prevents a port from becoming a Root Port by disabling it if superior BPDUs are received, thereby enforcing the current Root Bridge
	- Loop Guard
		- Protects the network from loops by disabling a port if it unexpectedly stops receiving BPDUs, ensuring it doesn't mistakenly enter the Forwarding state
### Portfast - The Problem
- When an end host connects to a switch port, the port becomes `up/up` but can't send/receive data yet
	- It is a **Designated port** but will take 30 seconds before it enters the **Forwarding** state:
		- 15 seconds in **Listening**
		- 15 seconds in **Learning**
- This leads to a poor user experience
	- The user probably doesn't even know STP exists
	- They just know "the internet doesn't work" for 30 seconds when they connect their computer
	- This wait is unnecessary, because there's no risk of a Layer 2 loops occurring between a switch/PC
### PortFast - The Solution
- When **PortFast** is configured on a port, the port immediately enters the **Forwarding** state when connected to another device
	- It bypasses **Listening/Learning** and can send/receive data right away
- PortFast can be configured in two ways:
	1. Interface config mode:
	  `SW1(config-if)# spanning-tree portfast`
	  This enables PortFast only on the individual interface
	2. Global config mode:
	  `SW1(config)# spanning-tree portfast default`
	  This enables PortFast on **all access ports**
- Connections between switches are almost always **trunk** links
- Connections to end hosts are almost always **access** links
### PortFast on Trunk Ports
- The standard PortFast configuration commands only enable PortFast on access ports
- In some cases, you might want to enable PortFast on a trunk port:
	- A port connected to a virtualization server with VMs in different VLANs
	- A port connected to a router via ROAS
- This can only be configured per-port in interface config mode:
	- `SW1(config-if)# spanning-tree portfast trunk`
### PortFast Edge
- PortFast edge and network are two versions of PortFast, but only edge is included in the CCNA exam topics list
- In modern Cisco switches, if you use the commands covered so far, the device will automatically add the **edge** keyword to the configuration
	- `SW1(config-if)# spanning-tree portfast`
	- In the running config: `spanning-tree portfast edge`
	- `SW1(config-if)# spanning-tree portfast trunk`
	- In the running config: `spanning-tree portfast edge trunk`
	- `SW1(config-if)# spanning-tree portfast default`
	- In the running config: `spanning-tree portfast edge default`
- Either version of the commands can be used when configuring PortFast
- The end result is the same: **edge** will always be added in the configuration
- `spanning-tree portfast disable` doesn't use the **edge** keyword
- To verify the **edge** keyword being used, you can use `show running-config interface [interface-id]` to view the running-config for the specified interface
- Unfortunately, this doesn't work in Packet Tracer