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