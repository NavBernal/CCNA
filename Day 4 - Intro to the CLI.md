# Intro to the CLI
### What is a CLI?
- Command-line interface
- The interface you use to configure Cisco devices
### How to connect to a Cisco device? (Console port)
- When you first configure a device, you have to connect to the console port
- The following Cisco switch shows two ways of connecting to the console port:
![](attachments/Pasted%20image%2020240906215131.png)
- To connect, you'd use a **Rollover cable** which is essentially an RJ45 to DVI-D adapter, like the following:
![](attachments/Pasted%20image%2020240906215239.png)
- Since DVI-D ports are no longer in use on most modern motherboards, a DVI-D to USB A adapter would need to be used alongside this
- Like a UTP cable, there are 8 pins to be used in a **Rollover cable**
- The pins connect in the following order:
![](attachments/Pasted%20image%2020240906215631.png)
- Once you've connected the computer to the device, you need to use a terminal emulator like **PuTTy** to access the Cisco CLI
- The following image shows how to connect to Cisco's CLI using the **Serial** connection type within PuTTy, as well as the default settings used:
![](attachments/Pasted%20image%2020240906220502.png)
- The default settings worth remembering for the exam are the following:
	- **Speed of 9600 bits per second**
	- **8 data bits**
	- **1 stop bit**
	- **No parity**
	- **No flow control**
- Once connected to the device, the following screen will pop up:
![](attachments/Pasted%20image%2020240906220727.png)
- When first entering the CLI, you'll be put into the **User EXEC Mode**
- This is indicated by the ">" sign next to the host name of the device
- The default host name for this devi