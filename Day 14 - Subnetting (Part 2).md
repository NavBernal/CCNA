# Subnetting (Part 2)
### Things We'll Cover
- Subnetting practice questions (Class C networks)
- Subnetting Class B networks
### Quiz From Part 1
![](attachments/Pasted%20image%2020241015194740.png)
- We know that the range for subnet 1 should be 192.168.1.0-192.168.1.63 (64 total addresses including the network and broadcast)
- Going off of the hint, the broadcast address for subnet 1 is 192.168.1.63
- This means that subnet 2 will begin with 192.168.1.64
- After doing the same for the other subnets, we end up with the following answers
	- Subnet 1 = 192.168.1.0
	- Subnet 2 = 192.168.1.64
	- Subnet 3 = 192.168.1.128
	- Suybnet 4 = 192.168.1.192
- Let's fact check:
![](attachments/Pasted%20image%2020241015195056.png)
![](attachments/Pasted%20image%2020241015195118.png)
![](attachments/Pasted%20image%2020241015195134.png)
![](attachments/Pasted%20image%2020241015195150.png)
![](attachments/Pasted%20image%2020241015195208.png)
- Notice that each new subnet adds 64 to the fourth octet
### Subnetting Trick
![](attachments/Pasted%20image%2020241015195303.png)
![](attachments/Pasted%20image%2020241015195341.png)
- When writing our the fourth octet in binary notation, notice that the last bit of the network portion is 64
- This means that to find the next subnet, we just have to add 64
- This same concept applies to all other subnets (/27 would mean adding 32 to each subnet)
### Subnetting Example
![](attachments/Pasted%20image%2020241015195511.png)
- In this case, the number of hosts for each subnet hasn't been specified
- Let's just make each subnet as large as they can be
- All we'd have to do is start with /24, and keep increasing the prefix length based on the total amount of allowed subnets for each borrowed bits
### /24
![](attachments/Pasted%20image%2020241015195645.png)
### /25
![](attachments/Pasted%20image%2020241015195747.png)
- We can see that adding a borrowed bit to our fourth octet essentially doubled that number of subnets we can use
- This is due to the formula shown above
- We still need 8 subnets, so using a /25 prefix length is still not enough
### /27
![](attachments/Pasted%20image%2020241015200005.png)
- Even though 8 is 3 more subnets than we need for the previous example, it's the best we can do as /26 would only allow us to create 4 subnets
- We can't always make the numbers match exactly to our needs, but that's ok to allow room for growth
![](attachments/Pasted%20image%2020241015200137.png)
![](attachments/Pasted%20image%2020241015200155.png)
- As shown here, the last bit of our network portion for a /27 subnet is 32
- This should be enough to calculate all 5 subnets:
	- Sub 1: 192.168.255.0/27
	- Sub 2: 192.168.255.32/27
	- Sub 3: 192.168.255.64/27
	- Sub 4: 192.168.255.96/27
	- Sub 5: 192.168.255.128/27
![](attachments/Pasted%20image%2020241015200358.png)
- If needed in the future, we still have 3 remaining subnets we can use
- Another possible question we can see on the exam is to identify the subnet an IP address belongs to
### Identify the Subnet
![](attachments/Pasted%20image%2020241015200516.png)
![](attachments/Pasted%20image%2020241015200632.png)
- The purple bits are "borrowed" and added to the network portion of our IP
- To figure out the network address, we simply need to change the remaining host bits to 0s
- As indicated by the hint in the first quiz question, the network address represents our subnet ID which in this case is 192.168.5.32
![](attachments/Pasted%20image%2020241015200731.png)
- We would repeat the same steps as the last problem
- I believe it's easier to simply focus on the host bits, which in this case would be **11011** 011
- The first 5 bits in bold represent our "borrowed" bits for the /29 subnet
- This means that we're allowed to change the last three to 0s, would turn our fourth octet to 216 (219 - (0 + 2 + 1))
- Subnet ID: **192.168.29.216**
![](attachments/Pasted%20image%2020241015201103.png)
### Subnets/Hosts (Class C)
![](attachments/Pasted%20image%2020241015201154.png)
- These numbers are worth memorizing in order to calculate subnets quicker
- For the number of subnets, each additional bit doubles our amount of total subnets
- The same concept applies for the numbers of hosts, the more host bits we have means the larger number of hosts for each subnet
- However, increasing the number of "borrowed" bits reduces our number of host bits
- As one gets larger, the other gets smaller and vice versa
- In the above chart, take note of /31