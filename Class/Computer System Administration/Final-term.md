# FTP
File Transfer Protocol
- client-server architecture
- separated control/data connections
	- control connection
		- created when FTP session is established
		- only for passing control information
	- data connection
		- a distinct TCP connection is established each time data is sent
		- Active/Passive mode
- security concern
	- FTP connections are transmitted in clear text
	- FTP over SSH
		- FTP through a SSH tunnel
	- SFTP
		- both commands and data are encrypted while transmitting
		- one connection, poor performance
	- FTP over TLS (FTPs)
		- Only commands are encrypted while transmitting
		- better performance
## Pure-FTPd
- support chroot
- restrictions on clients and system-wide
- verbose logging with `syslog`
- anonymous user
- virtual users and Unix authentication
- FXP (File eXchange Protocol)
- FTP over TLS
### 