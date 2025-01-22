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
- The Disabled state is