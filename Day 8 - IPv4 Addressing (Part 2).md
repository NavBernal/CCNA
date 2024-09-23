# IPv4 Addressing (Part 2)
### Things We'll Cover
- IPv4 address classes (review, clarification)
- Finding the:
	- max number of hosts
	- network address
	- broadcast address
	- first usable address
	- last usable address
- of a particular network
- Configuring IP addresses on Cisco devices
### IPv4 Address Classes
![](attachments/5a08edd71d37599fb281ea823bb209de.png)
- When it comes to Class A, different sources say different things regarding the range so it's best to stick to 0-127 to be safe
- But also keep in mind that 1 and 127 are reserved, so really the usable range is 126
![](attachments/a2cdff1c525289b5ea45ac8bf5e281b4.png)
### Max Hosts Per Network
- Class C:
![](attachments/f7c426dda9a4e69a177793f3cb6b58a7.png)
- Class B:
![](attachments/819e19f0a59e4f3f4fbad100a589da90.png)
- Class A:
![](attachments/59503f9db06db986c411560636dada69.png)
- Formula for calculating the max hosts is:
![](attachments/c7379a033c3ff54e87a400e8228306ef.png)
### First/Last Usable Addresses
- Class A:
![](attachments/28fee9f87076a7c59fa70828e58389f7.png)
- Class B:
![](attachments/c26771973d5400460f1c5e9d38f0769f.png)
- Class C:
![](attachments/9686d7d051f34b66a7809f5676fa48cb.png)
### IPv4 Addressing
- Diagram of our network with each Class type A-c
![](attachments/6dbd1e1a165cbf254d344a220b1804ad.png)
- CLI Configuration:
![](attachments/f06fae333f8483d4e3f47144d1038746.png)
- Column Explanations:
- **Interface:**
	- Lists the network interfaces on the device
- **IP-Address:**
	- Lists the IP address of each interface
	- They are all unassigned by default
- **OK?**
	- A legacy feature of the command and is no longer relevant
	- It basically says whether or not the IP address is valid, however, modern devices won't let you assign invalid IP addresses
	- The interfaces currently have no IP addresses assigned, which is considered a valid state
- **Method:**
	- Indicates the method by which the interface was assigned an IP address
	- Is set to 'unset' by default
- **Status:**
	- **Layer 1** status of the interface
	- If the interface is enabled, there is a cable connected, and the other end of the cable is properly connected to another device, you should see 'up' here
	- However, here it displays 'administratively down' which means the interface has been disabled with the 'shutdown' command
	- Since no configurations have been done on the interfaces yet, this can be considered the default status of Cisco router interfaces
	- Cisco switch interfaces are NOT administratively down by default
	- They'll either be up if connected to another device, or down if they're not connected
- **Protocol:**
	- This is the **Layer 2** status of the interface
	- Since the interfaces are down at Layer 1, Layer 2 can't operate, so all the interfaces are down at Layer 2
	- You'll never see an interface with a 'down' in the status column and 'up' in the protocol column, although the reverse is possible
	- Once we configure these interfaces and enable them, we should see 'up' in both the status and protocol columns
- **Worth Remembering:**
	- **Status** refers to the **Layer 1** status
	- **Protocol** refers to the **Layer 2** status
![](attachments/9d3a2f1cfc91c5e9d0c3ff700fd30d3d.png)
- Command to enter interface config mode: `interface [interface name]`
![](attachments/befef23f10cf773165405227da16e7f5.png)
- As shown in the `show ip interface brief` command, the GigEthernet0/0 interface has been properly assigned an IP as indicated by the changes in the corresponding columns
### `show interfaces [interface]`
![](attachments/11af65a7ccdf82442967d90d75fcb1e6.png)
- This command shows primarily Layer 1 and Layer 2 information about the interface, but also some Layer 3
- `GigabitEthernet0/0 is up` = Layer 1 is working
- `line protocol is up` = Layer 2 is working
- `Hardware is iGbe` = 1 Gigabit Ethernet
- `address is 0c1b.8444.f000` = MAC address
- `bia` = burned in address, it lists the address twice as this is the physical address on the device, but you can actually re-configure a different MAC address using the CLI
- `Internet address is 10.255.255.254/8` = IP address
### `show interfaces description`
![](attachments/3916fca08ced1361a8c34ee561d17dc6.png)
- Interface descriptions are optional, but can be very helpful in identifying the purpose of each interface
- To be able to add descriptions to an interface, we would do the following:
![](attachments/dbb7e9c91db1c2c85daf35bd7e4aa728.png)
- The command to configure an interface description is simply `description` followed by the description