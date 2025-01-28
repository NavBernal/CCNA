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
- **BPDU Guard** protects the network from unauthorized switches being connected to ports indended for end hosts
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
	- If you re-enable the port wihtout fixing the issue, it'll just be err-disabled again
- You can re-enable an err-disabled port in two ways:
	1. Manual: use `shutdown` and `no shutdown` to reset the disabled port
	2. Automatic: `ErrDisable Recovery`
### ErrDisable Recovery
- **ErrDisable Recovery** is a feature that automatically re-enables err-disabled ports after a certain period of time
- To see the current status of ErrDisable, use the command `show errdisable recovery`
- It's disabled by default
- The default recovery time is 300 seconds (5 minutes)
	- Err-disabled interfaces will be automatically re-eanbled after 5 minutes
- Use `SW1(config)# errdisable recovery interval [seconds]` to modify the interval
- Use `errdisable recovery cause [cause]` to enable ErrDisable Recovery for ports disabled by a particular cause
- If the cause is BPDU guard, we would use the command `errdisable recovery cause bpduguard`
