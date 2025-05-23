# OSI Model & TCP/IP Suite
### What is a networking model?
- Networking **models** categorize and provide a structure for networking **protocols** and standards
	- A protocol is a set of **logical** rules defining how network devices and software should work
### OSI (Open Systems Interconnection) Model
- A conceptual model that categorizes and standardizes the different functions in a network
- Created by the **International Organization for Standardization (ISO)**
- Functions are divided into 7 'layers'
- These layers work together to make the network work
![](attachments/ed40ce4bba3849619f81da9aec92194c.png)
### Layer 7 - Application Layer
- This layer is closes to the end user
- Interacts with software applications, for example, your web browser
- HTTP and HTTPS are Layer 7 protocols (**https**://www.cisco.com)
- Functions of Layer 7 include:
	- Identifying communication partners
	- Synchronizing communication
![](attachments/db12dd29567b0e510c4f0d920bfca48b.png)
- Let's pretend these two OSI models represent two systems looking to talk to each other through the application layer
- The software application, maybe a browser, interactors with Layer 7 (Application Layer) and wants to send some data to the system on the right
![](attachments/a5d397281ab9a14d910945b9bc164d0b.png)
- The data is process through the OSI stack, each layer adding something to the original data
- This is called **ENCAPSULATION** because the original data is encapsulated inside this additional information which is added on
- By the time it reaches the physical layer, it's electrical signals on a wire, and is set to the neighboring system
- Then, the neighboring system performs the opposite process
![](attachments/9f03536fa9adb91f2dc2647ecf0e031d.png)
- The additions of each layer are stripped off until the data reaches the application layer of the neighboring system
- This process is called **DE-ENCAPSULATION** as the additional information is removed as data is processed up the stack
![](attachments/257a770411061b29f31d513e8459e26c.png)
- Both the **encapsulation** and **de-encapsulation** processes are examples of **adjacent-layer interaction**, which is interaction between the different layers of the OSI model
![](attachments/72f518d929442a293b084f8f588da0d7.png)
- However, the communication between the application layers of the two different system is called **same-layer interaction**
- This **same-layer interaction** between application layers is what allows the application layer to perform its functions of identifying communication partners, synchronizing communications, etc.
### Layer 6 - Presentation Layer
- Data in the application layer is in 'application format'
- It needs to be 'translated' to a different format to be sent over the network
- The **Presentation** Layer's job is to translate between application and network formats
- For example, encryption of data as it is sent, and decryption of data as it is received
- Also translates between different Application-Layer formats to ensure that the data is in a format the receiving host can understand
- All you really need to know is that **the presentation layer translates data into the appropriate formation**
### Layer 5 - Session Layer
- Controls dialogs (sessions) between communicating hosts
- Establishes, manages, and terminates connections between the local application (your web browser) and the remote application (YouTube)
### OSI Model - The Upper Layers
- Network engineers don't usually work with the top 3 layers
- Application developers work with the top layers of the OSI model to connect their applications over networks
![](attachments/044918af4fa0004980dd73463aa77c1d.png)
- Data prepared at the top 3 layers is then sent over to the bottom 4 layers, which actually do the work of sending over the network
![](attachments/4897f91f73a251fbde1eae33bff9d1a1.png)
- After the top 3 layers hand data over to the bottom 4 layers, the next step before sending data is that **Layer 4, the Transport Layer,** adds a header in front of the data
### Layer 4 - Transport Layer
- Segments and reassembles data for communications between end hosts
- Breaks large pieces of data into smaller segments which can be more easily sent over the network and are less likely to cause transmission problems if errors occur
	- For example, if data wasn't segmented, and you were trying to watch a video, if an error occurred that prevented the video from reaching your computer, you wouldn't be able to watch the video at all
	- However, if the data is segmented into many small units, and only one fails to reach the destination, that's not a big problem
	- The video might skip for a second, but then will continue on just fine
- Provide **host-to-host** or **end-to-end** communication
![](attachments/4897f91f73a251fbde1eae33bff9d1a1.png)
- Data is prepared at the top 3 layers and a Layer 4 header is added on
- At this point in the process, this unit of data (plus Layer 4 header) is called a **segment**
- If the data being sent is large enough, it will actually be segmented into smaller parts, and a Layer 4 header will be added on to each segment
![](attachments/48af49590b0062709eb29820f2f3997c.png)
- Next, that segment is passed on to Layer 3, and another header is added on to the end
![](attachments/a3e2c7b56afe6657b9c53d7b38a02ab7.png)
### Layer 3 - Network Layer
- Provides connectivity between end hosts on different networks (i.e. outside the LAN)
- Provides logical addressing (IP addresses)
- Provides path selection between source and destination
	- Often, there are many possible paths which data can take to reach its destination, especially over a huge network like the Internet
	- Layer 3 provides the means of selecting the best path
- **Routers** operate at Layer 3
![](attachments/48af49590b0062709eb29820f2f3997c.png)
- To review the **encapsulation** process, we know that data is prepared in the upper 3 layers and send down to the transport layer which adds a Layer 4 header
- This combination of data plus layer 4 header is called a **segment**
![](attachments/03fab90b95953055596a0276ed3ceaeb.png)
- Next, the network layer adds a layer 3 header, including information like the source and destination IP address, to the segment
- This combination of data, layer 4 header, and layer 3 header, is called a **packet**
![](attachments/346cf529726c6b3bbb7edfea4922a15a.png)
- Next, the packet is further encapsulated at Layer 2, this time with both a **Layer 2 header AND a Layer 2 trailer**
### Layer 2 - Data Link Layer
- Provides node-to-node connectivity and data transfer (i.e. PC to switch, switch to router, router to router)
- Defines how data is formatted for transmission over a physical medium (i.e. copper UTP cables)
- Detects and (possibly) corrects Physical Layer errors
- Uses Layer 2 addressing, separate from Layer 3 addressing
- Switches operate at Layer 2
	- Switches look at the destination Layer 2 address to determine where to send the data
![](attachments/65e85d1d31da2e59ed2d8cbf454e534d.png)
- At this point in the **encapsulation** process, the combination of data, layer 4 header, layer 3 header, layer 2 header and trailer is called a **frame**
- The data is not further encapsulated over layer 1
- The **frame** is sent over the connection, whether it's electrical signals over a wire or wireless signals in the case of Wi-Fi, to the neighboring system
### Layer 1 - Physical Layer
- Defines physical characteristics of the medium used to transfer data between the devices
- For instance, voltage levels, maximum transmission distances, physical connectors, cable specifications, etc.
- Digital bits are converted into electrical signals (for wired connections) or radio signals (for wireless connections)
- All the information in the [Day 02 - Interfaces & Cables](Day%2002%20-%20Interfaces%20&%20Cables.md)) lecture (cables, pin layouts, etc.) is related to the **Physical Layer**
![](attachments/5370674e8fac3c0ebce8521b8d5f788e.png)
- Now we've got a complete **frame**, and that **frame** will be sent from the local device over this cable (i.e. Ethernet cable)
![](attachments/587e2ae137e92c805ddc4fa488cbb6cd.png)
- Once it reaches the remote device, the reverse process of encapsulation, **de-encapsulation** takes place
- The data link layer translates the raw physical data into a complete frame once again
![](attachments/3d5bf8d641b712ecee0cf0ab7673ae82.png)
- Then the layer 2 header and trailer are removed, leaving the **layer 3 packet**
![](attachments/9cd3aef8940b3fea12138c863b20650b.png)
- The layer 3 header is removed, leaving the **layer 4 segment**
![](attachments/60d80a96a8c56c8d0b6eda795a94dfc5.png)
- Finally, the layer 4 header is removed, and we are left with the original data prepared by the upper layers of the original device
- That's the process of **de-encapsulation**
### PDUs
![](attachments/01d2bfab31ef1be4c3972221b011cd88.png)
- **Segment** is the term for a **Layer 4** PDU, etc.
- At **Layer 1**, the PDU is a **bit** referring to the bits being transferred on the wire
### OSI Model Acronyms
![](attachments/3491f5086d90bf75812e09db2d635f2f.png)
### TCP/IP Suite
- Conceptual model and set of communications protocols used in the Internet and other networks
- Known as TCP/IP because those are two of the foundational protocols in the suite
- Developed by the United States Department of Defense through DARPA (Defense Advanced Research Projects Agency)
- Similar structure to the OSI Model, but with fewer layers
- This is the model actually in use in modern networks
- The OSI model still influences how network engineers think and talk about networks
### OSI vs TCP/IP
![](attachments/302ed2e7ef15093065b4e33c4c012a82.png)
- This graph shows which layers of the OSI Model correlate to the same layers on the TCP/IP Suite
- It's worth mentioning, however, that when talking about networks, we use the OSI numbering
- For instance, if a Network Engineer is talking about a "layer 4 network problem", they're referring to the Transport Layer of the OSI model, not the Application Layer of the TCP/IP model
### TCP/IP Suite
![](attachments/0dc140dc5554550f0fe7387bc63377c5.png)
- This diagram demonstrates the process of a host, Host A, sending data to Host B, with two routers in between
- This is known as **same-layer interaction**
- The transport layer provides **host-to-host** communications
- This transport layer segment was never changed during this whole process
- Since TCP/IP protocols are all industry standard protocols used by all makers, it doesn't matter what kind of PC or router you're using
- Any computer can communicate with any router, signifying the importance of having industry standards