# Network Time Protocol
### Things We'll Cover
- Why is time important for network devices?
- Manual time configuration
- NTP basics/configuration
### The Importance of Time
- In Cisco IOS, you can view the time with the `show clock` command
	- The default time zone is UTC (Coordinated Universal Time)
- If you use the `show clock detail` command, you can see the time source
	- `*` = time is not considered authoritative
	- The hardware calendar is the default time source
- The internal hardware clock of a device will drift over time, so it's not the ideal time source
- The most important reason to have accurate time on a device is to **have accurate logs for troubleshooting**
- **Syslog**, the protocol used to keep device logs, will be covered in a later lecture
- `show logging` is the command used to view Cisco logs
### Manual Time Configuration
- `show clock`
- `show clock detail`
- `clock set (hh:mm:ss) {day|month} {month|day} (year)`: Manually configure the time on the device
- Although the hardware calendar (built-in clock) is the default time-source, the hardware clock and software clock are separate and can be configured separately
### Hardware Clock (Calendar) Configuration
- `show calendar`
- `calendar set (hh:mm:ss) {day|month} {month|day} (year)` : Manually configure the hardware clock
- Typically, you will want to sync the clock and calendar
- `clock update-calendar`: Syncs the calendar to the clock's time
- `clock read-calendar` Syncs the clock to the calendar's time
### Configure the Time Zone
- `R1(config)# clock timezone (name) (hours-offset) [minutes-offset]`
### Daylight Saving Time (Summer Time)
- `R1(config)# clock summer-time recurring (name) (start) (end) [ooffset]`
### Network Time Protocol
- Manually configuring the time on devices is not scalable
- The manually configured clocks will drift, resulting in inaccurate time
- **NTP (Network Time Protocol)** allows automatic syncing of time over a network
	- i.e. syncing with the server `time.windows.com`
- NTP clients request the time from NTP servers
	- A device can be both at the same time
- Allows accuracy of time within 1 millisecond if the NTP sever is in the same LAN
- Or within 50 milliseconds if connecting to the NTP server over a WAN/the Internet
- Some NTP servers are 'better' than others
	- The 'distance' of an NTP server from the original **reference clock** is called **stratum**
- NTP uses **UDP port 123** to communicate
### Reference Clocks
- A reference clock is usually a very accurate time device like an atomic clock or a GPS clock
- Reference clocks are **stratum 0** within the NTP hierarchy
- NTP servers directly connected to reference clocks are **stratum 1**
### NTP Hierarchy
- Reference clocks are **stratum 0**
- **Stratum 1** NTP servers get their time from reference clocks
- **Stratum 2** NTP servers get their time from stratum 1 NTP servers
- **Stratum 15** is the maximum
	- Anything above that is considered unreliable
- Devices can also 'peer' with devices at the same stratum to provide more accurate time
	- This is called **symmetric active** mode
- Cisco devices can operate in three NTP modes:
	- Server
	- Client
	- Symmetric active
	- Can be in all three at the same time
- An NTP client can sync to multiple NTP servers
- NTP servers which get their time directly from reference clocks are called **primary servers**
- NTP servers which get their time from other NTP servers are called **secondary servers**
	- They operate in server mode and client mode at the same time
### NTP Command Review
**Basic Configuration Commands**
- `R1(config)#`
	- `ntp server (ip-address) [prefer]`
	- `ntp peer (ip-address)`
	- `ntp update-calendar`
	- `ntp master [stratum]`
	- `ntp source (interface)`
**Basic Show Commands**
- `R1#`
	- `show ntp associations`
	- `show ntp status`
**Basic Authentication Commands**
- `R1(config)#`
	- `ntp authenticate`
	- `ntp authentication-key (key-number) md5 (key)`
	- `ntp trusted-key (key-number)`
	- `ntp server (ip-address) key (key-number)`
	- `ntp peer (ip-address) key (key-number)`
