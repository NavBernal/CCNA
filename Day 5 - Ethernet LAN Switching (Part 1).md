# Ethernet LAN Switching (Part 1)
### OSI Model - Physical Layer
- Defines physical characteristics of the medium used to transfer data between devices
- For example, voltage levels, maximum transmission distances, physical connectors, cable specs, etc.
- Digital bits are converted into electrical signals (for wired connections) or radio signals (for wireless connections)
- All of the information in the [Day 2 - Interfaces & Cables](Day%202%20-%20Interfaces%20&%20Cables.md) lecture is related to the Physical Layer
### Data Link Layer
- Provides node-to-node connectivity and data transfer (PC to switch, switch to router, etc.)
- Defines how data is formatted for transmission over a physical medium (copper UTP cables)
- Detects and (possibly) correctly Physical Layer errors
- Uses Layer 2 addressing (MAC address), separate form Layer 3 addressing (IP address)
- Switches operate at Layer 2
### Local Area Networks (LANs)
- A network contained in a relatively small area
- Routers are used to connect seperate LANs, as noted in the following diagram:
![](attachments/Pasted%20image%2020240907225558.png)
- Switches don't separate LANs, but adding more switches can be used to expand an existing LAN
- This means that, despite containing two switches, the red devices are all apart of one large LAN
- The blue devices are the same ones used in the red LAN, but instead of the switches being connected to each other, they're connected to different router interfaces
- That's why they're considered as being two separate LANs
### PDUs
![](attachments/01d2bfab31ef1be4c3972221b011cd88.png)
- This lesson will go over how switches receive and forward frames, specifically **Ethernet frames**, since it's the Layer 2 protocol used in virtually all LANs in existence today
### Ethernet Frame
![](attachments/Pasted%20image%2020240907230347.png)
#### Ethernet Header
- **SFDs (Start Frame Delimiters)** are used for synchornization and to allow the receiving device to be prepared to receive the rest of the data in the frame
- Next is the **Destination**, which is the Layer 2 address to which the frame is being sent
- The **Source** is the Layer 2 address of the device which sent the frame
- Finally, the **Type** indicates the Layer 3 protocol used in the encapsulated Packet, which is almost always IPv4 or IPv6
- However, sometimes this is a length field, indicating the legnth of the encapsulated data, depending on the version of Ethernet
- The Ethernet trailer
#### Ethernet Trailer
- **FCS (Frame Check Sequence)** is used by the receiving device to detect any errors that might have occured in transmission
### Preamble & SFD
![](attachments/Pasted%20image%2020240907230505.png)
### Destination & Source
![](attachments/Pasted%20image%2020240907230611.png)
### Type/Length
![](attachments/Pasted%20image%2020240907230826.png)
### Ethernet Header
- 