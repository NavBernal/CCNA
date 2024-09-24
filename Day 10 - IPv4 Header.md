# IPv4 Header
### Things We'll Cover
- IPv4 packet structure
- Fields of the IPv4 header
### OSI Model - PDUs
![](attachments/Pasted%20image%2020240923140926.png)
### IPv4 Header
![](attachments/Pasted%20image%2020240923141031.png)
### Version Field
- Length: 4 bits
- Identifies the version of IP used
- IPv4 = 4 (0100)
- IPv6 = 6 (0110)
### Internet Header Length (IHL)
- Length: 4 bits
- The final field of the IPv4 header (options) is variable in length, so the field is necessary to indicate the total length of the header
- Identifies the length of the header in **4-byte increments**
- Value of 5 = 5 x 4-bytes = 20 bytes
- Minimum value is 5 (= 20 bytes)
- Maximum value is 15 (15 x 4-bytes = 60 bytes)
![](attachments/Pasted%20image%2020240923145457.png)
- **Minimum IPv4 Header Length = 20 bytes**
- **Maximum IPv4 Header Length = 60 bytes**
### DSCP
- Differentiated Services Code Point
- Length: 6 bits
- Used for QoS (Quality of Service)
- Used to prioritize delay-sensitive data (streaming voice, video, etc.)
### ECN Field
- Explicit Congestion Notification
- Length: 2 bits
- Provides end-to-end (between two endpoints) notification of network congestion **without dropping packets**
- Optional feature that requires both endpoints, as well as the underlying network infrastructure, to support it
### Total Length Field
- Length: 16 bits
- Indicates the total length of the packet (L3 header + L4 segment)
- Measured in bytes (**not 4-byte increments like IHL)
- Minimum value of 20 (=IPv4 header with no encapsulated data)
- Maximum value of 65,535 (maximum 16-bit value)
![](attachments/Pasted%20image%2020240923221453.png)
### Identification Field
- If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to
- All fragments of the same packet will have their own IPv4 header with the same value in this field
- Packets are fragmented if larger than the **MTU** (Maximum Transmission Unit)
- The MTU is usually **1500 bytes**
- This is equivalent to the maximum size of an Ethernet frame
- Fragments are reassembled by the receiving host
### Flags Field
- Used ot control/identify fragments
- Bit 0: Reserved, always set to 0
- Bit 1: Don't Fragment (DF Bit), used to indicate a packet that should not be fragmented
- Bit 2: More Fragments (MF bit), set to 1 if there are more fragments in the packet, set to 0 for the last fragment
- Unfragmented packets will always have their MF bit set to 0
### Fragment Offset Field
- Length: 13 bits
- Used to indicate the position of the fragment within the original, unfragmented IP packet
- Allows fragmented packets to be reassembled even if the fragments arrive out of order
### Time to Live Field
- Length: 8 bits
- A router will drop a packet with a TTL of 0
- Used to prevent infinite loops
- originally designed to indicate the packet's maximum lifetime in seconds
- In practice, indicates a 'hop count': each time the packet arrives at a router, the router decreases the TTL by 1
- Recommended default TTL is 64
### Protocol Field
- Length: 8 bits
- Indicates the protocol of the encapsulated L4PDU
- Value of 6: TCP
- Value of 17: UDP
- Value of 1: ICMP
- Value of 89: OSPF (dynamic routing protocol)
### Header Checksum Field
- Length: 16 bits
- A calculated checksum used to check for errors in the IPv4 header
- When a router receives a packet, it calculates the checksum of the header and compares it to the one in this field of the header
- If they do not match, the router drops the packet
- Used to check for errors only in the IPv4 header
- IP relies on the encapusulated protocol to detect errors in the encapsulated data
- Both TCP and UDP have their own checksum fields to detect errors in the encapsulated data
### Source/Destination IP Address Fields
- Length: 32 bits
- Source IP Address = IPv4 address of the sender of the packet
- Destination IP Address = IPv4 address of the intended receiver of the packet
### Options Field
- Length: 0-320 bits
- Rarely used
- If the IHL field is greater than 5, it means that Options are present