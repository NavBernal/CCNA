# Quality of Service (Part 1)
### Things We'll Cover
- IP Phones/Voice VLANs
- Power over Ethernet (PoE)
- Intro to Quality of Service (QoS)
### IP Phones
- Traditional phones operate over the *public switched telephone network* (PSTN)
- Sometimes this is called POTS (Plain Old Telephone Service)
- IP Phone use VoIP technologies to enable phone calls over an IP network, such as the Internet
- IP phones are connected to a switch just like any other end host
- IP phones have an internal 3-port switch
	- 1 port is the 'uplink' to the external switch
	- 1 port is the 'downlink' to the PC
	- 1 port connects internally to the phone itself
- This allows the PC and the IP phone to share a single switch port
- Traffic from the PC passes through the IP phone to the switch
- It's recommended to separate 'voice' traffic (from the IP phone) and 'data' traffic (from the PC) by placing them in separate VLANs
	- This can be accomplished using a *voice VLAN*
	- Traffic from the PC will be untagged, but traffic from the phone will be tagged with a VLAN ID
### IP Phones/Voice VLAN
![](attachments/1b241220fd416087cac6a39e43fc7fb9.png)
### Power over Ethernet (PoE)
- PoE allows Power Sourcing Equipment (PSE) to provide power to Powered Devices (PD) over an Ethernet cable
- Typically the PSE is a switch and the PDs are IP phones, IP cameras, wireless access points, etc.
- The PSE receives AC power from the outlet, converts it to DC Power, and supplies that DC power to the PDs
![](attachments/797b6471404ab08f3af08802b96f207a.png)
- Too much electrical current can damage electrical devices
- PoE has a process to determine if a connected device needs power, and how much it needs
	- When a device is connected to a PoE-enabled port, the PSE (switch) sends low power signals, monitors the response, and determines how much power the PD needs
	- If the device needs power, the PSE supplies the power to allow the PD to boot
	- The PSE continues to monitor the PD and supply the required amount of power (but not too much!)
- *Power policing* can be configured to prevent a PD from taking too much power
	- `power inline police` configures power policing with the default settings: disable the port and send a Syslog message if a PD draws too much power
		- equivalent to `power inline police action err-disable`
		- the interface will be put in an 'error-disabled' state and can be re-enabled with `shutdown` followed by `no shutdown`
	- `power inline police action log` doesn't shut down the interface if the PD draws too much power
		- it will restart the interface and send a Syslog message
![](attachments/ffbde033fd6fe4659168643a0813ef13.png)
### Quality of Service (QoS)
- Voice traffic and data traffic used to use entirely separate networks
	- Voice traffic used the PSTN
	- Data traffic used the IP network (enterprise WAN, Internet, etc.)
- QoS wasn't necessary as the different kinds of traffic didn't compete for bandwidth
- Modern networks are typically *converged networks* in which IP phones, video traffic, regular data traffic, etc. all share the same IP network
- This enabled cost savings as well as more advanced features for voice and video traffic
	- i.e. integrations with collaboration software (Cisco WebEx, MS Teams, etc.)
- However, different kinds of traffic now have to compete for bandwidth
- QoS is a set of tools used by the network devices to apply different treatment to different packets
- QoS is used to manage the following characteristics of network traffic:
	1. **Bandwidth**
		- The overall capacity of the link, measured in bits per second (Gbps, Mbps, etc.)
		- QoS tools allow you to reserve a certain amount of a link's bandwidth for specific kinds of traffic
		- For instance: 20% voice traffic, 30% for specific kinds of data traffic, and 50% for all other traffic
	2. **Delay**
		- **one-way delay:** The amount of time it takes traffic to go from source to destination
		- **two-way delay:** The amount of time it takes traffic to go from source to destination and return
	3. **Jitter**
		- The variation in one-way delay between packets sent by the same application
		- IP phones have a 'jitter buffer' to provide a fixed delay to audio packets
	4. **Loss**
		- The % of packets sent that don't reach their destination
		- Can be caused by faulty cables
		- Can also be caused when a device's packet *queues* get full and the device starts discarding packets
- The following standards are recommended for acceptable interactive audio (i.e. phone call) quality:
	- **One-way delay:** 150 ms or less
	- **Jitter:** 30 ms or less
	- **Loss:** 1% or less
- If these standards aren't met, there could be a noticeable reduction in the quality of the phone call
### QoS - Queuing
- If a network device receives messages faster than it can forward them out of the appropriate interface, the messages are placed in a queue
- By default, queued messages will be forwarded in a FIFO manner
	- Sent in the order they're received
- If the queue is full, new packets will be dropped
	- This is called *tail drop*
![](attachments/f87f5b0b06937f99f4e6c5b9d9d8e60b.png)
- **Tail drop** is harmful because it can lead to **TCP global synchronization**
- Review of the **TCP sliding window:**
	- Hosts using TCP use the 'sliding window' increase/decrease the rate at which they send traffic as needed
	- When a packet is dropped, it'll be re-transmitted
	- When a drop occurs, the sender will reduce the rate it sends traffic
	- It'll then gradually increase the rate again
- When the queue fills up and **tail drop** occurs, all TCP hosts sending traffic will slow down the rate at which they send traffic
- They'll all then increase the rate at which they send traffic, which rapidly leads to more congestion, dropped packets, and the process repeats
![](attachments/c5f580a49580c3e1bf22dbbb432a7a02.png)
- A solution to prevent tail drop and TCP global synchronization is **Random Early Detection (RED)**
- When the amount of traffic in the queue reaches a certain threshold, the device will start randomly dropping packets from select TCP flows
- Those TCP flows that dropped packets will reduce the rate at which traffic is sent, but you'll avoid global TCP synchronization, in which ALL TCP flows reduce and then increase the rate of transmission at the same time in waves
- In standard RED, all kinds of traffic are treated the same
- An improved version, **Weighted Random Early Detection (WRED)**, allows you to control which packets are dropped depending on the traffic class