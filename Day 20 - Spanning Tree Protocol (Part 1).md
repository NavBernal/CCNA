# Spanning Tree Protocol (Part 1)
### Things We'll Cover
- Redundancy in networks
- STP (SPanning Tee Protocol)
### Network Redundancy
- Redundancy is an essential part of network design
- Modern networks are expected to run 24/7/365. Even a short downtime can be disastrous for a business
- If one network component fails, you must ensure that other components will take over with little or no downtime
- As much as possible, you must implement redundancy at every possible point in the network
- Most PCs only have a single network interface card (NIC), so they can only be plugged into a single switch
- However, important servers typically have multiple NICs, so they can be plugged into multiple switches for redundancy
### Broadcast Storms
- The Ethernet header doesn't have a TTL field
- These broadcast frames will loop around the network indefinitely
- If enough of these looped broadcasts accumulate in the network, the network will be too congested for legitimate traffic to use the network
- This is called a **broadcast storm**
- Network congestion isn't the only problem
- Each time a frame arrives on a switchport, the switch uses the source MAC address field to 'learn' the MAC address and update its MAC address table
- When frames with the same source MAC address repeatedly arrive on different interfaces, the switch is continuously updating the interface in its MAC address table
- This is known as **MAC Address Flapping**
### Spanning Tree Protocol
- 'Classic Spanning Tree Protocol' is IEEE 802.1D
- Switches from ALL vendors run STP by default
- STP prevents Layer 2 loops by placing redundant ports in a blocking stae, essentially disabling the interface
- These interfaces act as backups that can enter a forwarding state if an active (=currently forwardng) interface fails
- Interfaces in a forwarding state behave normally, they send and receive all normal traffic
- Interfaces in a blocking state only send or receive STP messages (called BPDUs - Bridge Protocol Data Units)
	- Spanning Tree Protocol still uses the term 'bridge'
	- However, when we use the term 'bridge', we really mean 'switch'
	- Bridges aren't used in modern networks
- By selecting which ports are **forwarding** and which ports are **blocking**, STP creates a single path to/from each point in the network
- This prevents L2 loops
- There is a set process that STP uses to determine which ports should be forwarding and which should be blocking
- STP-enabled switches send/receive Hello BPDUs out of all interfaces, the default time is 2 seconds (the switch will send a Hello BPDU out of every interface, once every 2 seconds)
- If a switch receives a Hello BPDU onan interface, it knows that interface is connected to another switch (routers, PCs, etc. don't use STP, so they don't send Hello BPDUs)
- Switches use one field in the STP BPDU, the **Bridge ID** field, to elect a **root bridge** for the network
- the switch with the lowest **Bridge ID** becomes the **root bridge**
- ALL ports on the **root bridge** are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge
#### Classic Bridge ID
![](attachments/Pasted%20image%2020250114203557.png)
- The default bridge priority is 32768 on all switches, so by default the MAC address is used as the tie-breaker (lowest MAC address becomes the root bridge)
	- The Bridge Priority is compared first
	- If they tie, the MAC address is then compared
#### Updated Bridge ID