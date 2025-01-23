# Spanning Tree Protocol (Part 2)
### Things We'll Cover
- STP states/timers
- STP BPDU
- STP optional features
- STP configuration
### Spanning Tree Port States
![](attachments/Pasted%20image%2020250122112759.png)
- Root/Designated ports remain stable in a **Forwarding** state
- Non-designated ports remain stable in a **Blocking** state
- **Listening** and **Learning** are transitional states which are passed through when an interface is activated, or when a **Blocking** port must transition to a **Forwarding** state due to a change in the network topology
#### Blocking State
- Non-designated ports are in a **blocking** state
- Interfaces in a Blocking state: 
	- Are effectively disabled to prevent loops
	- Don't send/receive regular network traffic
	- Receive STP BPDUs
	- Do NOT forward STP BPDUs
	- Do NOT learn MAC addresses
#### Listening State
- After the Blocking state, interfaces with the Designated or Root role enter the **Listening** state
- Only **Designated** or **Root** ports enter the Listening state (Non-designated ports are always Blocking)
- The Listening state is 15 seconds long by default. This is determined by the **Forward delay** timer
- An interface in the Listening state:
	- ONLY forwards/receives STP BPDUs
	- Does NOT send/receive regular traffic
	- Does NOT learn MAC addresses from regular traffic that arrives on the interface
#### Learning State
- After the Listening state, a Designated or Root port will enter the **Learning** state
- The Learning state is 15 seconds long by defualt. This is determined by the **Forwarding delay** timer (the same timer is used for both the Listening and Learning states)
- An interface in the Learning sate:
	- ONLY sends/receives STP BPDUs
	- Does NOT send/receive regular traffic
	- **DOES learn** MAC addresses from regular traffic that arrives on the interface
#### Forwarding State
- Root and Designated ports are in a **Forwarding** state
- A port in the Forwarding state:
	- Operates as normal
	- Sends/receives BPDUs
	- Sends/receives normal traffic
	- Learns MAC addresses
![](attachments/Pasted%20image%2020250122121216.png)
- The Disabled state is the spanning tree state of a shutdown, administratively disabled, interface
### Spanning Tree Timers
![](attachments/Pasted%20image%2020250122164636.png)
#### Hello STP Timer
- Switches do not forward the BPDUs out of their **root** ports and **non-designated** ports, only their **designated** ports
#### Max Age Timer
- If another BPDU is received before the max age timer counts down to 0, the time will reset to 20 seconds and no changes will occur
- If another BPDU isn't received, the max age timer counts down to 0 and the switch will reevaluate its STP choices, including root bridge, and local root, designated, and non-designated ports
- If a non-designated port is selected to become a designated or root port, it'll transition from the blocking state to the listening state (15 seconds), learning state (15 seconds), and then finally the forwarding state
- So, it can take a total of **50 seconds** for a blocking interface to transition to forwarding
- These timers and transitional states are to make sure that loops aren't accidentally created by an interface moving to forwarding state too son
- A forwarding interface can move directly to a blocking state (there is no worry about creating a loop by blocking an interface)
- A blocking state cannot move directly to forwarding state. It must go through the listening and learning states
### Spanning Tree BPDU
- Cisco's PVST+ uses the destination MAC address of `01:00:0c:cc:cc:cd` for its BPDUs, **THIS IS WORTH REMEMBERING FOR THE TEST**
- **PVST** = Only ISL trunk encapsulation
- **PVST+** = Supports 802.1Q
- Regular STP (not Cisco's PVST+) uses a destination MAC address of `0180.c200.0000`
- The STP timers on the root bridge determine the STP timers for the entire network
### Spanning Tree Optional Features (STP Toolkit)
#### Portfast
- Portfast allows a port to move immediately to the **Forwarding** state, bypassing **Listening** and **Learning**
- If used, it must be enabled **only on ports connected to end hosts**
- If enabled on a port connected to another switch, it could cause a Layer 2 loop
- Portfast is enabled on an interface using the command `spanning-tree portfast`
- Portfast can also be abled using the command `spanning-tree portfast default`
- This enables portfast on **all access ports** (not trunk ports)
#### BPDU Guard
- If an interface with BPDU Guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loop from forming
- This can be configured using the command `spaning-tree bpduguard enable`
- It can also be enabled using the command `spanning-tree portfast bpduguard default`
- This enables BPDU Guard on all Portfast enabled interfaces
#### Root Guard
- If you enable **root guard** on an interface, even if it receives a superior BPDU (lower bridge ID) on that interface, the switch will not accept the new switch as the root bridge
- The interface will be disabled
#### Loop Guard
- If you enable **loop guard** on an interface, even if the interface stops receiving BPDUs, it will not start forwarding
- The interface will be disabled
### Configure the Spanning Tree Mode
- `spanning-tree mode ?`
	- `mst = Multiple spanning tree mode`
	- `pvst = Per-Vlan spanning tree mode`
	- `rapid-pvsts = Per-vlan rapid spanning tree mode`
### Configure the Primary Root Bridge
- To configure the root bridge, we can use the `spanning-tree vlan [vlan-id] root primary` command
- This command sets the STP priority to 24576
- If another switch already has a priority lower than 24576, it sets this switch's priority to 4096 less than the other switch's priority
- The command that is actually applied in this case is `spanning-tree vlan [vlan-id] priority 24576`
### Configure the Secondary Root Bridge
- The command to set the secondary root bridge is `spanning-tree vlan [vlan-id] root secondary`
- This command sets the STP priority to 28672
### Configure STP Port Settings
- `spanning-tree vlan [vlan-id] ?`
	- `cost = Change an interface's per VLAN spanning tree path cost`
	- `port-priority = Change an interface's spanning tree port priority`