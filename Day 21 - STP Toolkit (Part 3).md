# Root Guard
### Root Bridge Replacement
- STP prevents loops by electing a root bridge and ensuring that all other switches have only **one valid path** to reach it
- The root bridge shouldn't be chosen randomly, instead, consider the following:
	- Optimal traffic flow
	- Stability and reliability
### Root Guard - The Problem
- Within your own LAN, you can easily control the root bridge by setting its priority to **0**
	- But there are cases where you might connect your LAN to other switches outside your direct control
- Even if you set your root bridge's priority to 0, its role can be taken by another switch with a lower MAC address
- **Root Guard** can be configured to protect your STP topology by preventing your switches from accepting **superior BPDUs** from switches outside your control
	- **Superior BPDU** = a BPDU that is superior in the STP algorithm (i.e. claiming a better root bridge ID)
### Root Guard - The Solution
- If you want to ensure that the root bridge remains in your LAN, you can configure **Root Guard** on the ports connected to switches outside your control
- Use `SW1(config-if)# spanning-tree guard root` to enable Root Guard on a port
	- There is no command to enable it by default from global config mode
- If a Root Guard-enabled port receives a BPDU, it will enter the **Broken** (Root Inconsistent) state, effectively disabling it
	- The port will not be able to forward data frames and will discard any frames it receives
- To re-enable a port disabled by Root Guard, you must solve the issue that disabled the port
	- The disabled port must stop receiving superior BPDUs
- Once the superior BPDUs received by the problem switches age out, the ports will automatically be re-enabled
	- A BPDUs Max Age is 20 seconds by default