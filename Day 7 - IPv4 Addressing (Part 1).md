# IPv4 Addressing (Part 1)
### Review: Network Layer
- Provides connectivity between end hosts on different networks (i.e. outside of the LAN)
- Provides logical addressing (IP addresses)
- Provides path selection between source and destination
- Routers operate at Layer 3
### Routing
![](attachments/761763d6812c13c7c45fe6d3fb85eb53.png)
- In the past lectures, we learned that if SW1 and SW2 were connected to each other, all devices in the diagram would be considered as being part of a single network
- However, with the inclusion of a router between both switches, we would now consider these as being two separate networks
![](attachments/56c1f0bd550fc7995dfab5e401016842.png)
- Each network is assigned its own IP address
- The first 3 groups of numbers, 192.168.1 & 192.168.2, represent the network itself, and only the last 0 changes to represent the end hosts on the network
- The /24 at the end is used to tell what part of the address represents the network, and which part represents the end hosts
- /24 means that the first 3 groups of numbers represents the network
- The router also needs two separate IP addresses that would connect to each network, which in this case will be 192.168.1.254 & 192.168.2.254
![](attachments/4f65392def6b15cd14591af72d893706.png)
- This time, if R1 sends a frame to the broadcast MAC address of all Fs, SW1 will receive the frame, and it will forward it out of all interfaces except the one the frame was received on
- So, it sends the frame out of G0/1 and G0/2, and PC2 and R1 receive the frame
- However, that's where it ends
- The broadcast is limited to the local network, it doesn't cross the router and go to SW2, PC3, & PC4
### IPv4 Header
![](attachments/d702b44234313285dafaca3851bce030.png)
- Both the source & destination IP addresses are both 32 bits in length, and stretch from 0-31 in this chart
- So, IP addresses are 32-bits or 4 bytes in length
### IPv4 Address
![](attachments/6ddd904d195a8ea20c312d8499d368a8.png)
### Decimal & Hexadecimal
![](attachments/d1077d49b64cb830dcbd0e7f650533b9.png)
### Binary (Base 2)
![](attachments/389ec4a7770b773d2d134eafe1e8d0c1.png)
- It's worth mentioning that we normally refer to each of these 8 bit groups as 'octets'
### Binary -> Decimal (1)
- First, it helps to write out the value of each binary digit over the binary numbers
- You can start with 1 on the right, and then multiple by 2 for each digit as you move to the left
- Or start by writing 128 over the digital to the left and dividing by 2 as you move to the right
- Once you've written these values, simple add up the value of each 1, and you'll get the decimal conversion
![](attachments/b2a8419a65c834d5163e2b7f0afd1ff5.png)
