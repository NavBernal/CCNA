# VLANs (Part 1)
### Things We'll Cover
- What is a LAN?
- Broadcast domains
- What is a VLAN?
- What is the purpose of VLANs?
- How to configure VLANs on Cisco switches
### What is a LAN?
- Basic definition: A LAN is a group of devices (PCs, servers, routers, etc.) in a single location (home, office, etc.)
- More specific definition: A LAN is a single **broadcast domain**, including all devices in that broadcast domain
- A **broadcast domain** is the group of devices which will receive a broadcast frame (destination MAC FFFF.FFFF.FFFF) sent by any one of the members
### LANs/Broadcast Domains
![](attachments/ec88b683894db206a9907231ed213715.png)
- Since R1 doesn't forward the broadcast frame to other devices, only PC1, PC2, SW1 and R1 are considered as being a single **broadcast domain**
- Now, what if PC1 sends a broadcast frame?
- Which devices will receive it?
![](attachments/5549bd286bdb31446479a8954374ab2b.png)
- It's pretty easy to figure out what will happen if PC6 sends out a broadcast frame
![](attachments/87f5e4657dffd5d5ec05f6c33a29671f.png)
- We've found three broadcast frames so far, however, there is one more
- What if R1 sends a broadcast frame out of its interface which is connected to R2?
![](attachments/0b0f8f6c36527481b515d9a22c0b94a4.png)
- Even though it will only be received by R1, this would still be considered its own broadcast domain
- In total, there are four broadcast domains in this network and therefore four LANs
### What is a VLAN?
![](attachments/e1ff846d12a1a681ee82122b8ccda77a.png)
- Here is a small LAN of a company
- Let's say there are three departments:
![](attachments/26d9b0b7e484f70c7c3a65bd51353247.png)
- In this example, all departments are own the same network address
- However, this isn't necessarily the best setup
- For both security and performance purposes, it would be best to split these up into separate subnets
- For example, let's say a PC in the engineering department sends a broadcast message intended for other PCs in the engineering department
![](attachments/96c5ae09eb6d158b033769f86f054914.png)
- Since it's a broadcast message, the switch will flood this message out to all the other PCs connected to it regardless of the department
![](attachments/74917820dedf096138bdaefcde4860f9.png)
- All PCs, as well as the router, will receive the broadcast
- This is a problem, for both security and network performance purposes
- **Performance:** Lots of unnecessary broadcast traffic can reduce network performance
- We should always minimize unnecessary traffic in our network
- **Security:**
	- Even within the same office, you want to limit who has access to what. You can apply security policies on a router/firewall.
	- Because this is one LAN, PCs can reach each other directly, without traffic passing through the router
	- So, even if you configure security policies, they won't have any effect
- We should separate these hosts so we can apply security policies that determine who can access what in the network
- So, lets split up these departments into separate subnets:
![](attachments/8d2cb0f82022fa8d52662862bd52455b.png)
- However, there's one problem with this setup
- The router is going to need an IP address in each subnet, so it will need one interface in each subnet
- So, let's replace the single connection between the switch and router with three separate connections, one in each subnet:
![](attachments/7ad324860906ff6acbc79a5cf5528075.png)
- There's actually a more efficient way of doing this where you don't have to use three interfaces, but that will be covered later
![](attachments/1e8e3ed9421c463089587a96404c5418.png)
- If PC1 sends some data to PC2, PC1 will recognize that PC2 is in a different subnet than its own
- It will set the destination MAC address to its default gateway, R1
- Here's what the frame would look like:
![](attachments/043b36c1b8a735cc270ba71c6a77dfa4.png)
- PC1 will forward the frame to the switch, which will send it to R1, which will then change the source MAC to its own MAC, and the destination MAC to PC2's MAC
![](attachments/e93decba4d6dd32a88bcdfed7e2d9034.png)
- New Frame:
![](attachments/4fbe90c6c91582d671829e8f44781dab.png)
- It will then forward the frame back to the switch, which will then forward it to the destination, PC2
![](attachments/c0ed6f7fd1a41b65c1d8865cb8832d60.png)
- So, instead of PC1 being able to send traffic directly to PC2, we forced it to send the traffic through R1 first
- We would have configured some security policies in and such in R1 to control exactly what traffic is allowed to pass between these subnets
- However, there is still a problem
- What if the frame is a broadcast or unknown unicast frame?
- The switch will flood the frame out of all interfaces
- For example, here's a broadcast frame:
![](attachments/cd78b3848737b3dc34ef895b14e491cc.png)
- This is a broadcast frame intended for the engineering department
- Remember that a switch is only aware up to Layer 2 meaning it only looks at Layer 2 information like source/destination MAC addresses
- It doesn't care about Layer 3, 4, etc.
- This means that even though there are three separate subnets here, the switch doesn't know that
- PC1 will send the frame to the switch, it will see the destination MAC address of all Fs, and then flood the frame like it did before
![](attachments/ab6535c2ca70a3a1e8124048eaf3f412.png)
- Although we separated the three departments into three subnets (Layer 3), they are still in the same broadcast domain (Layer 2)
- One possible solution is to buy a separate switch for each department, however, that's not very flexible, and netork equipment isn't cheap
- Buying one or more switches for every single department could be too expensive, especially for a small business
- This is where **VLANs** come in
- Although these PCs are all in the same LAN, we can use VLANs to separate them at Layer 2
- We'll assign the following departments to their respective VLANs:
	- Engineering: VLAN10
	- HR: VLAN20
	- Sales: VLAN30
![](attachments/02cd80dc298b23c6126b31e3c2f39fe4.png)
- In order to assign these hosts to VLANs, they must be configured on the switch
- More specifically, on the switch interfaces
- You configure the switch interface to be in a specific VLAN, and then the end host connected to that interface is part of that VLAN
- The switch will consider each VLAN as a separate LAN, and **WILL NOT** forward traffic between VLANs, including **broadcast/unkown unicast** traffic
- So, if PC1 sends the same broadcast frame as before, after the frame arrives at the switch, it will be forwarded to **all interfaces in the same VLAN**
![](attachments/36b3cb36024a365c35c1ec95b53a72a4.png)
- Since the broadcast arrived at an interface configured in VLAN10, the switch will only forward the frame to other interfaces in VLAN10
- However, if PC1 wants to send the same unicast frame to PC2, it will function just like before
- The switch does not perform **inter-VLAN routing**. It must send the traffic through the router.
- Notice, traffic that arrives at the VLAN10 interface is forwarded out of a VLAN10 interface
![](attachments/b409d6bdc3336cf2658df8018c89c0d7.png)
- Also, traffic that arrives at the VLAN30 interface is forwarded out of the VLAN30 interface
- Both in the same VLAN
![](attachments/da7f9dcea69e2d6f1767154c9ab8dfc9.png)
- A switch will never forward traffic directly between two VLANs like this:
![](attachments/20d4b3a2de2709324824ef59e095aa05.png)
- First, the two hosts are in separate subnets, so PC1 itself will send the traffic to its default gateway, R1
- However, even if PC1 and PC2 were in the same subnet, the switch wouldn't forward the traffic from PC1 to PC2, because they are in separate VLANs
### Review
- VLANs...
	- Are configured on switches on a **per-interface** basis
	- **logically** separate end hosts at Layer 2
- Switches do not forward traffic directly between hosts in different VLANs
### VLAN Configuration
![](attachments/af2334e2a392d6b516691b41dc7d59cc.png)
- The diagram now contains the interfaces for all the VLANs
	- VLAN10: G1/0 through G1/3
	- VLAN2: G2/0 through G2/2
	- VLAN30: G3/0 through G3/3
- Here's how to add these changes using the Cisco CLI:
![](attachments/369cfd90a37bdc6045d80480f44c044d.png)
- Before configuration, let's look at the VLANs that exist by default on a switch using the `show vlan brief` command
- This command displays the VLANs that exist on the switch, and which interfaces are in each VLAN
- Here, you can see VLAN1, with the name `default`
![](attachments/4b1291933e62fc8bc4f5ec0b545cbeed.png)
- This is the VLAN that all interfaces are assigned to by default
- So, even if you don't configure any VLANs, all interfaces are in VLAN1 by default
- Under `Ports`, you can see all the interfaces on this device, from G/0 to G3/3
- Under it are four other VLANs, 1002 to 1005, used for FDDI and token ring:
![](attachments/0ebb34ea929e01fe25b0fd50ab1fcc86.png)
- These are old technologies that won't be necessary for the CCNA
- VLANS 1, 1002-1005 exist by default and **cannot be deleted**
- This is how you assign interfaces to a VLAN:
![](attachments/fdfe617d80d3c5794a0a2e621e68afb4.png)
- First, use the `interface range` command to configure all the VLAN10 interfaces at once
- Use the `switchport mode access` command to set the interface as an **access port**
	- An **access port** is a switchport which belongs to a single VLAN, and usually connects to end hosts like PCs
	- That's why it's called an **access port**, it gives the end hosts **access** to the network
- There another important type of switchport called a **trunk port**
	- Switchports which carry multiple VLANs are called **trunk ports**
	- This will be covered more in depth in the next lecture
- A switchport connected to an end host should enter access mode by default
- However, it's always a good idea to explicitly configure the setting and not rely on autonegotation of port type
- The last command used is `switchport access vlan 10`, which is the command that actually assigns the VLAN to the port
- Notice the message that appears after this command: `Access VLAN does not exist. Creating vlan 10`
- Since VLAN10 didn't exist on the device yet, it was created automatically when we assigned the interface to VLAN10
- The same steps are used to configure all the VLAN20 and VLAN30 interfaces:
![](attachments/2759cc96d72f1f28c800b2e2384fbc94.png)
![](attachments/866af14e8b25391b80cd8ff85296f9bb.png)
- When running the `show vlan brief` command once again, we can now see the three VLANs that were configured using the previous commands:
![](attachments/15b0044d33de1cad6501499257ca15e8.png)
![](attachments/a5c8265f117d64f5d5d1af077e79a821.png)
- Notice the default names of each VLAN, those can be changed to be more understandable
![](attachments/aa02ff71bf7c542a90eaa74a8d124121.png)
- To enter configuration mode for VLAN10, we simply use the `vlan 10` command
- This is the same command used to create a VLAN, however in this case, it was already automatically created when we assigned the interfaces
- Next, the name can be assigned using the simple `name <enter name here>` command
- Now if we run the `show vlan brief` command again, we can see the changes we made
![](attachments/64500ca863a175923dca7564f4546af0.png)
- That's all for the configurations
- Now, if the command `ping 255.255.255.255` is used on PC1 (this command sends a ping with the destination MAC address of all Fs), the broadcast will only reach hosts in VLAN10
![](attachments/5eaa95c301f25104db6443f9b235b863.png)
- Likewise, if the same command is used on PC2, the broadcast will only reach PCs in VLAN30
![](attachments/e6c03c337c6e771848eb01e39800d1d4.png)
### Things we Covered
- What is a LAN?
- Broadcast domains
- What is a VLAN?
	- Essentially a way to logically split up a Layer 2 broadcast domain, to make multiple separate broadcast domains
- What is the purpose of a VLAN?
	- Network & Security performance
	- Help to reduce unnecessary broadcast traffic, which helps prevent network congestion and therefore improve network performance
	- Limiting broadcast and unknown unicast traffic like this also improves network security, since these messages won't be received by devices outside the VLAN
	- You should always make sure that network traffic isn't sent unnecessarily to other devices as much as possible
- How to configure VLANs on Cisco switches
	- Configured access ports on a Cisco switch and assigned them to a specific VLAN
### Quiz Question 1
How many broadcast domains are shown in this network diagram?
![](attachments/9e84569a190385a3f34f1d953fd40c33.png)
- **Answer: 6**
![](attachments/e745d4cc02f6ecbec353760f564fe7ef.png)
- Each router interface and everything connected to it are in one broadcast domain, since no VLANs have been configured
### Quiz Question 2
How many broadcast domains are shown in this network diagram?
![](attachments/0939a41a1c77d27a37f37bf61ae0b2fd.png)
- **Answer: 5**
![](attachments/88af3db7fc4411ca2a68cda66f9efc2b.png)
- One for each configured VLAN, and the connection between two routers is also a broadcast domain
### Quiz Question 3
What happens if you try to assign a switch interface to a VLAN that doesn't exist?
![](attachments/24c5604e8a167ff406836921ad6cc125.png)
- **Answer: B**
- As shown earlier in the video, if you assign a switch interface to a VLAN that doesn't exist yet, the switch will create the VLAN automatically
![](attachments/3d19dfc21a9053930d960b6dab22d9be.png)
### Quiz Question 4
If PC3 sends a broadcast message, how many devices will receive it?
![](attachments/11d938f6ed9f5905d7f85173a9d777d4.png)
**Answer: 3**
![](attachments/17b6f935bc665f4e415a19857c90b4c5.png)
- First, the switch will receive it
- Then, it will send it out of all interfaces in VLAN20, so the router and the other PC in the VLAN
- This makes a total of 3 devices
- If no VLANs were configured, ALL other PCs would receive it
- But since we have configured VLANs, only these devices in the same VLAN will receive it
### Quiz Question 5
You create VLANs 10, 20, and 30 on a Cisco switch. How many VLANs will be displayed in the output of the `show vlan brief` command?
- a) 3
- b) 5
- c) 8
- d) 10
**Answer: 8**
- I answered incorrectly and said 3
![](attachments/f6af4978e378fd0fb278bbb40ac46eed.png)
- VLANs 1, 1002, 1003, 1004, and 1005 exist by default and cannot be deleted
- So, if you create three additional VLANs, there will be a total of 8 VLANs on the switch