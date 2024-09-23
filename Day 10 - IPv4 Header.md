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