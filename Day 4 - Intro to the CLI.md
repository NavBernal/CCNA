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
### User EXEC Mode
- When first entering the CLI, you'll be put into the **User EXEC Mode**
- This is indicated by the ">" sign next to the host name of the device
- The default host name for this device is **Router**
![](attachments/Pasted%20image%2020240906221017.png)
- All devices have a host name, and for a Cisco router the default name is **Router**
- User EXEC mode is very limited, as it only allows users to view things but not write any changes
- It's also called **user mode** for short
### Privileged EXEC Mode
- If you enter the `enable` command in user EXEC mode, you will be placed into **privileged EXEC mode**, and then a "#" is displayed instead of a ">" sign next to the host name
![](attachments/Pasted%20image%2020240906221309.png)
- **Privileged EXEC mode** provides complete access to view the device's configuration, restart the device, etc.
- You cannot change the configuration just yet, but you can change the time on the device, save the configuration file, etc.
- Here's a comparison of the commands available in **User EXEC Mode vs Privileged EXEC Mode**:
![](attachments/Pasted%20image%2020240906221521.png)
- These screenshots have been captured using **Cisco's Packet Tracer Software** and may not actually display all of the available commands that would show on a physical Cisco device, but it's good enough to get the point across
- As noted in the screenshots, you can use a "?" symbol to view the available commands that you currently have access to
- In Cisco CLI, you can save time entering complete commands by using `Tab` to complete the command
- However, you can save even more time by just typing the beginning of the command and pressing `Enter` as shown here:
![](attachments/Pasted%20image%2020240906221946.png)
- This only works because `enable` is the only command beginning with `en`
- Trying to only type `e` by itself would not work as there are other commands beginning with `e` such as `erase` or `exit`
- An easy way to view all commands beginning with a specific letter would be to type the letter followed by the "?" sign, so typing in `e?` would display all available commands beginning with `e`
### Global Configuration Mode
- In order to enter this mode, we would enter the command `configure terminal` in the CLI
- When in this mode, config is inserted after the hostname: `Router(config)#`
- Like before, you don't have to type in the whole word and can get away with using something like `conf t` instead
### Enable Password
- We can protect **User EXEC Mode** with a password so that when the `enable` command is entered, the user would be forced to enter the password before getting any further
- This is done with the command `enable password` when inside **Global Configuration Mode**
- Another good trick to remember is that while using "?" next to a command will give us all possible commands containing that word or letter, including a space in between will give us all possible options for that command
- So something like `password ?` will give us all possible options for the `password` command, as shown here:
![](attachments/Pasted%20image%2020240906223223.png)
- It's also worth mentioning that passwords **ARE** case-sensitive within Cisco CLI, so trying to use `ccna` the log in wouldn't work in this case since `CCNA` was set
- The password also does **NOT** display as you type it, and would instead appear blank in the CLI even though your input is going through like normal
- If you enter the wrong password 3 times, you'll be denied access for having `bad secrets`
![](attachments/Pasted%20image%2020240906223553.png)
### Running-Config/Startup-Config
- There are two separate config files on the device at once
- 