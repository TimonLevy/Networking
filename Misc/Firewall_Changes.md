# FW RULE CHANGES 2.0
```
RULE 4	UserToFileServer		[Changed]
[+] Made destination address the File Server instead of whole segment.
-----------------------------------------------------------------------------------------------------------------------------
Rule 5	UsersToInternet			[Changed]
[+] Added support for QUIC.
-----------------------------------------------------------------------------------------------------------------------------
RULE 8	UsersToWeb			[CHANGED]
[-] Removed SMB protocol access.
[-] Removed SSH protocol access.
[-] Removed FTP protocol access.
[+] Added accurate destination addresses. (ip range of apache machines, WP1 and IIS)
-----------------------------------------------------------------------------------------------------------------------------
RULE 9	UsersToFTP			[Created]
[+] Added FTP protocol access.
[+] Added accurate destination addresses. (DMZ FTP servers)
-----------------------------------------------------------------------------------------------------------------------------
RULE 10	UsersToDB			[Disabled]
[?] Users shouldn't have direct access to company databases.
	They can use web servers and other interfaces to interact with it.
-----------------------------------------------------------------------------------------------------------------------------
RULE 11 UsersToDMZ			[Disabled]
[?] Users shouldn't have unlimited access to a whole segment in the network.
[+] Added more pinpoint rules.
-----------------------------------------------------------------------------------------------------------------------------
RULE 14	UsersToDNS			[Created]
[+] Allow DNS access to the dns servers.
-----------------------------------------------------------------------------------------------------------------------------
RULE 15	UsersToSIEM-Management
[-] Removed RDP protocol access.
[-] Removed SSH protocol access.
-----------------------------------------------------------------------------------------------------------------------------
RULE 16 UsersToCeontrify		[Created]
[+] Allow LDAP access to join Linux machines to the domain.
-----------------------------------------------------------------------------------------------------------------------------
RULE 17 UsersToHMI			[Disabled]
[-] Users shouldn't have access to the HMI.
-----------------------------------------------------------------------------------------------------------------------------
RULE 23 User management			[CHANGED]
[+] Changed any destination protocols to the following list:
	- CIFS (protocol group)
	- pop-3
	- IMAP
	- SMTP
	- MSExchange
	- DHCP
	- DNS
	- RPC
	- RPC/EPM
-----------------------------------------------------------------------------------------------------------------------------
```

![Picture](./FW_changes_image.png)
