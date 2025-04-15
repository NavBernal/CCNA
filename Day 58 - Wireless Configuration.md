# Wireless Configuration
### Things We'll Cover
- Topology introduction
- Switch configuration
- WLC setup
- WLC interface configuration
- WLAN configuration
- Additional WLC features
### Network Topology
![](attachments/a04f3fc48164733654f68fa0ba2b98de.png)
![](attachments/d3ae43033f037fc9e16cfd62e5c6a168.png)
![](attachments/32be3d87ec5c593a0820fb0bba6f9fbf.png)
### Switch Configuration
![](attachments/138b42ce64087e2b5e3b937272a3df5d.png)
![](attachments/9e94258d89a5cacfdef088f5d5565f02.png)
### WLC Initial Setup
![](attachments/3262aa195bbc9a5387d034bff937f126.png)
- The options in all caps (i.e. NO) will be chosen as the default if you hit enter
![](attachments/266b73607152aa33dab001910285b4df.png)
![](attachments/c9a6a7df002df3212140d2f8ef6fe303.png)
### Accessing the GUI
![](attachments/193cb309542f4d263217ba6f65f0ee3c.png)
### WLC Ports/Interfaces
- WLC **ports** are the physical ports that cables connect to
- WLC **interfaces** are the logical interfaces within the WLC (i.e. SVIs on a switch)
- WLCs have a few different kinds of **ports**:
	- **Service port:** 
		- A dedicated management port
		- Used for out-of-band management
		- Must connect to a switch access port because it only supports one VLAN
		- This port can be used to connect the device while it's booting, performing system recovery, etc.
	- **Distribution system port:**
		- These are the standard network ports that connected to the 'distribution system' (wired network) and are used for data traffic
		- These ports usually connect to switch trunk ports, and if multiple distribution ports are used they can form a LAG
	- **Console port:**
		- This is a standard console port, either RJ45 or USB
	- **Redundancy port:**
		- This port is used to connect to another WLC to form a high availability (HA) pair
![](attachments/8b3efef7c94c289472c0231c3309a04e.png)
- WLC have a few different kinds of **interfaces:**
	- **Management interface:**
		- Used for management traffic such as Telnet, SSH, HTTP, HTTPS, RADIUS authentication, NTP, Syslog, etc.
		- CAPWAP tunnels are also formed to/from the WLC's management interface
	- **Redundancy management interface:**
		- When two WLCs are connected by their redundancy ports, one WLC is 'active' and the other is 'standby'
		- This interface can be used to connect to and manage the 'standby' WLC
	- **Virtual interface:**
		- This interface is used when communicating with wireless clients to relay DHCP requests, perform client web authentication, etc.
	- **Service port interface:**
		- If the service port is used, this interface is bound to it and used for out-of-band management
	- **Dynamic interface:**
		- These are the interfaces used to map a WLAN to a VLAN
		- For example, traffic from the 'Internal' WLAN will be sent to the wired network from the WLC's 'Internal' dynamic interface
### WLC Configuration
**Interfaces:**
![](attachments/e43a841cd69b9b048b30bb7a30be7f0b.png)
![](attachments/d4894a812b4e0c35556c6ac1d6d4d47c.png)
![](attachments/6661c93238f7d392a90660aee127bd6d.png)
![](attachments/8de28d986194f2c2570e0b770621562b.png)
![](attachments/5a436d02353d989bf663061a32e7b3a4.png)
#### Internal WLAN
![](attachments/b92d9bcf51036679859e4f30c80015da.png)
![](attachments/1e5dab6f868b53414f48b1f541f7997c.png)
![](attachments/decd095dc133ef70dc92081284e88dd5.png)
![](attachments/57a737de9b04fa812d424d9359d32a9b.png)
**Following L3 security options aren't needed for the CCNA:**
![](attachments/9012b7e58d08d8be72765f7785d40fd8.png)
- **Web Authentication:**
	- After the wireless clients get an IP address and try to access a web page, they'll have to enter a username and password to authenticate
- **Web Passthrough:**
	- Similar to the above, but no username or password are required
	- A warning or statement is displayed, and the client simply has to agree to gain access to the Internet
- The **Conditional** and **Splash Page** web redirect options are similar, but additionally require 802.1X layer 2 authentication
![](attachments/d880291119b7b7c18c43245e93bdab70.png)
![](attachments/5202822881ebd06f9b28a91e19a40f53.png)
![](attachments/d941af57a8a48546db3f7fd86f37ac7b.png)
#### Guest WLAN
![](attachments/5ec8accf2865b3272e1266e11bf769c2.png)
![](attachments/19df065da6f0dfaf043543af9e1281d9.png)
- Profile name and SSID don't have to be the same
![](attachments/c4286fcbbe4aac91a70fd8cf0cf43513.png)
- Click on the checkbox to enable
- Check the interface group to 'guest'
- The only other change that has to be made is to change the security policy to use PSK instead of 802.1X
- There are a decent amount of steps to create/apply an ACL in the WLC management GUI, but the following is important:
![](attachments/96c98fca2d0b98cad20df3a4c54b4b26.png)
