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
![](attachments/Pasted%20image%2020241101163940.png)
- Since R1 doesn't forward the broadcast frame to other devices, only PC1, PC2, SW1 and R1 are considered as being a single **broadcast domain**
- Now, what if PC1 sends a broadcast frame?
- Which devices will receive it?
![](attachments/Pasted%20image%2020241101165343.png)
- It's pretty easy to figure out what will happen if PC6 sends out a broadcast frame
![](attachments/Pasted%20image%2020241101165421.png)
- We've found three broadcast frames so far, however, there is one more
- What if R1 sends a broadcast frame out of its interface which is connected to R2?
![](attachments/Pasted%20image%2020241101165545.png)
- Even though it will only be received by R1, this would still be considered its own broadcast domain