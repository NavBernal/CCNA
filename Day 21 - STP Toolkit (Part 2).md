# BPDU Guard & BPDU FIlter
###  PortFast & BPDUs
- **PortFast** makes a port start in the Forwarding state when it's connected, but it doesn't disable STP on the port
	- The port will continue to send BPDUs every 2 seconds
- Because end hosts don't run STP and send BPDUs, a PortFast-enabled port shouldn't receive BPDUs
	- But what if it does?
- If a PortFast-enabled port receives an STP BPDU, it will revert to acting like a regular STP port (without PortFast)
### BPDU Guard - The Problem
- PortFast should only be enabled on ports connected to non-switch devices (end hosts, routers)
	- These devices don't send BPDUs
- A PortFast-enabled port still sends BPDUs and will operate like a regular STP port if it receives BPDUs from a neighbor
- If an end user carelessly connects a switch to a port meant for end hosts, it could affect the STP topology
	- **BPDU Guard** acts as a safeguard against this
### BPDU Guard - The Solution
- **BPDU Guard** protects the network from unauthorized switches being connected to ports intended for end hosts
- It can be configured separately from **PortFast**, but both features are usually used together
	- They both enhance STP's functionality on ports intended for end hosts
- A BPDU Guard-enabled port continues to send BPDUs, but if it receives a BPDU it enters the **error-disabled** state
	- In effect, this disables the port
### BPDU Guard Configuration
- Like **PortFast, BPDU Guard** can be configured in two ways:
	- Per-port: `SW1(config-if)# spanning-tree bpduguard enable`
	- Default: `SW1(config)# spanning-tree portfast bpduguard default`
		- When enabled by default, **BPDU Guard** is activated on **all PortFast-enabled ports, not all access ports**
		- Use `spanning-tree bpduguard disable` in interface config mode to disable it on specific ports
### ErrDisable
- **ErrDisable** is a Cisco switch feature that disables a port under certain conditions, such as a BPDU Guard violation
- A few others that will be on the exam include:
	- Power Policing violations
	- Port Security violations
	- DAI (Dynamic ARP Inspection) violations
- To re-enable an err-disabled port, **first solve the underlying issue**
	- If you re-enable the port without fixing the issue, it'll just be err-disabled again
- You can re-enable an err-disabled port in two ways:
	1. Manual: use `shutdown` and `no shutdown` to reset the disabled port
	2. Automatic: `ErrDisable Recovery`
### ErrDisable Recovery
- **ErrDisable Recovery** is a feature that automatically re-enables err-disabled ports after a certain period of time
- To see the current status of ErrDisable, use the command `show errdisable recovery`
- It's disabled by default
- The default recovery time is 300 seconds (5 minutes)
	- Err-disabled interfaces will be automatically re-enabled after 5 minutes
- Use `SW1(config)# errdisable recovery interval [seconds]` to modify the interval
- Use `errdisable recovery cause [cause]` to enable ErrDisable Recovery for ports disabled by a particular cause
- If the cause is BPDU guard, we would use the command `errdisable recovery cause bpduguard`
### BPDU Filter - The Problem
- A switch port connected to an end host continues sending BPDUs every 2 seconds, regardless of whether PortFast and/or BPDU Guard are enabled
- If the port doesn't connect to a switch, sending BPDUs is unnecessary and undesirable for a couple of reasons
	1. Sending BPDUs uses some bandwidth and processing power on the switch (although it's minimal)
	2. BPDUs contain information about the LAN's STP topology
		- If maximum security is a concern, this info shouldn't be sent to user devices
- **BPDU Filter** solves this by preventing a port from sending BPDUs
### BPDU Filter - The Solution
- Unlike BPDU Guard, **BPDU Filter** doesn't disable the port if it receives a BPDU
- It can be enabled in two ways:
	- Per-port: `SW1(config-if)# spanning-tree bpdufilter enable`
		- The port will not send BPDUs and will ignore any it receives
		- In effect, this disables STP on the port
		- **USE WITH CAUTION**, not having STP can cause the whole network to go down
	- Default: `SW1(config)# spanning-tree portfast bpdufilter disable`
		- BPDU Filter will be activated on **all PortFast-enabled ports**
			- You can use `spanning-tree bpdufilter disable` to disable it on specific ports
		- The port will not send BPDUs
		- If the port receives one, PortFast and BPDU Filter are disabled, and it operates as a normal STP port
- Jeremy's Recommendation:
	- Enable PortFast and BPDU GUard however you prefer (per-port or by default)
	- However, only enable BPDU Filter by default (global config mode)
- **BPDU Guard** and **BPDU Filter** can be enabled on the same port at the same time:
	- If BPDU FIlter is enabled in **global config mode** and the port receives a BPDU:
		1. BPDU filter will be disabled
		2. BPDU Guard will be triggered (and err-disable the interface)
	- If BPDU Filter is enabled in **interface config mode** and the port receives a BPDU:
		- The BPDU will be ignored
		- BPDU Guard will **not** be triggered