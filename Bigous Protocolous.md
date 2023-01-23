# BIGOUS PROTOCOLOUS

The big protocol wikipedia.

| TABLE OF CONTENTS | _________________________________________________________________________________________________________________ |
| :-------------------------------------------------------------------- | ------------------------------------------------------------: |
| [ICMP](#internet-control-message-protocol-aka-icup)                   | Internet Control Message protocol                             |
| [DNS](#domain-name-system-aka-dns)                                    | Domain Name System                                            |
| [LLMNR](#link-local-multicast-name-resolution-aka-llmnr)              | Link-Local Multicast Name Resolution                          |
| [DHCP](#dynamic-host-configuration-protocol-aka-dhcp)                 | Dynamic Host Configuration Protocol                           |
| [NetBIOS](#network-basic-inputoutput-system-aka-netbios)              | Network Basic Input Output System                             |
| [Telnet](#teletype-network-aka-telnet)                                | Teletype Network                                              |
| [FTP](#file-transfer-protocol-aka-ftp)                                | File Tranfer Protocol                                         |
| [TFTP](#trivial-file-transfer-protocol-aka-tftp)                      | Trivial File Tranfer Protocol                                 |
| [HTTP](#hypertext-transfer-protocol-aka-http)                         | Hypertext Tranfer Protocol                                    |
| [SMTP](#simple-mail-transfer-protocol-aka-smtp)                       | Simple Mail Tranfer Protocol                                  |
| [POP](#post-office-protocol-3-aka-pop3)                               | Post Office Protocol                                          |
| [IMAP](#internet-message-access-protocol-aka-imap)                    | Internet Message Access Protocol                              |
| [SMB](#server-message-blocking-protocol-aka-smb)                      | Server Message Blocking                                       |
| [SNMP](#simple-network-management-protocol-aka-snmp)                  | Simple Network Management Protocol                            |
| [NTP](#network-time-protocol-aka-ntp)                                 | Network Time Protocol                                         |
| [TLS/SSL](#transport-layer-security--secure-socket-layer-aka-tlsssl)  | Transport Layer Security / Secure Socket Layer                |
| > [TLS 1.2 - Handshake Protocol](#handshake-protocol-ssl-30---tls-12) | SSL 3.0 - TLS 1.2 Handshake Protocol                          |
| > [Change Cipher Protocol](#change-cipher-spec-protocol)              | SSL Change Cipher Protocol                                    |
| > [TLS 1.3 - Handshake Protocol](#handshake-protocol-tls-13)          | TLS 1.3 Handshake Protocol                                    |
| > [Alert Protocol](#alert-protocol)                                   | TLS/SSL Alert Protocol                                        |
| > [Record Protocol](#record-prtocol)                                  | TLS/SSL Record Layer Protocol                                 |
| [HTTPS](#hypertext-transfer-protocol-over-tlsssl-aka-https)           | Hypertext Tranfer Protocol over TLS/SSL                       |
| [IPsec](#internet-protocol-security-aka-ipsec)                        | Internet Protocol Security Suite                              |
| > [IKEv1.0](#ikev10)                                                  | Internet Key Exchange Protocol ver 1.0                        |
| > [IKEv2.0](#ikev20)                                                  | Internet Key Exchange Protocol ver 2.0                        |
| > [AH Protocol](#authenticationi-header-ah-protocol)                  | Authentication Header Protocol                                |
| > [ESP](#encapsulation-security-protocol-esp)                         | Encapsulation Security Protocol                               |
| [SSH](#secure-shell-aka-ssh)                                          | Secure Shell                                                  |
| > [SSH Transport Layer Protocol](#ssh-transport-layer-protocol)       | SSH Transport Layer Protocol                                  |
|> [SSH User Authentication Protocol](#ssh-user-authentication-protocol)| SSH User Authentication Protocol                              |
| > [SSH Communication Protocol](#ssh-connection-protocol)              | SSH Communication Protocol                                    |
| [RDP](#remote-desktop-protocol-aka-rdp)                               | Remote Desktop Protocol                                       |
| [MS-SSTDS](#microsoft-sql-server-tubular-data-stream-aka-ms-sstds)    | Microsoft SQL Server Tubular Data Stream (MSSQL)              |
| [Modbus](#modbus)                                                     | Modbus                                                        |

[to Bibliography](#bibliography)

---------------------------------------------------------------------------------------------------------------------------------------------------

## Internet Control Message Protocol A.K.A [ICUP](www.urbandictionary.com/define.php?term=Spell%20iCup)
###### *[#Layer-3] [#Portless]*

ICMP, or **I**nternet **C**ontrol **M**essage **P**rotocol is a protocol used to send control messages relating to the Internet protocol (IP).

### WHY WAS IT INVENTED
Since the implementation of IP is not meant to be absolutely reliable and it can sometimes mess up, The ICMP protocol be used to return send feedback about problems that can arise when communicatinf with a host. However the ICMP protocol does not act to make the IP more reliable and does not guarantee 100% reliablility itself incase IP fails to deliver. ICMP will usually report on delivery errors of packets and no ICMP packet will be sent about a lost ICMP packet.

### HOW DOES IT WORK
An ICMP packet is used to instruct a device in the routing path of a packet (including the source and destination) of a failure or change in the delivery of the packet (Except for `ICMP Type`s 0 and 8). The type and structure of the packet is determined by the `ICMP Type` parameter in the header. Each type also has an `ICMP Code` that determines the subject of the packet.

### ICMP TYPE SPECIFICATION EXAMPLES

#### `[0 & 8] Echo & Echo Reply`

An ICMP Echo is used to check the connectivity from one endpoint to another, and ICMP Echo Reply is used to reply to Echo requests. Endpoints can send an `ICMP Echo Request` to another endpoint which will get routed to them and if the destination machine recieves the request it will send back an `ICMP Echo Reply`.

> #### `Types`
> |   |                                     |
> |---|-------------------------------------|
> | 8 | Echo Request                        |
> | 1 | Echo Reply                          |

> #### `Code`
>
> 0 (Default value)

#### `[3] Destination Unreachable`

An ICMP `Destination Unreachable` message will be recieved when a packet can't be delievered because a waypoint couldn't route it, the destination host service could not accept the packet (IP protocol or port isn't active), the gateway determined that the destination is unreachable or a DF (Dont Fragment) interuppted a fragmented packet and it was dropped due to it.

> #### `Type`
>
> 3

> #### `Codes`
>
> |   |                                     |
> |---|-------------------------------------|
> | 0 | net unreachable                     |
> | 1 | host unreachable                    |
> | 2 | protocol unreachable                |
> | 3 | port unreachable                    |
> | 4 | Fragmantation needed and DF was set |
> | 6 | Source route failed                 |
> 
> Codes 0, 1, 4 and 5 can be recieved from a gateway. Codes 2 and 3 can be recieved from a host.

#### `[11] Time Exceeded`

> An ICMP `Time Exceeded` if a packet's TTL reaches 0 while traveling to it's destination, the final waypoint it encoutered may send a Time Exceeded message to the sender host, or, if the desination host cannot reassemble a fragmented packet in the time limit it may send a Time Exceeded to the sender host.

> #### `Type`
> 
> 11

> #### `Codes`
> 
> |   |                                     |
> |---|-------------------------------------|
> | 0 | ttl exceeded in transit             |
> | 1 | fragment reassembly time exceeded   |
>
> Code 0 can be sent from a waypoint, code 1 can be sent from a host.



###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Domain Name System A.K.A DNS
###### *[#Layer-7] [#TCP/53] [#UDP/53]*

The DNS protocol is used for "translating" between IP addresses and Domain Names, thus the name Domain Name System. The protocol itself operates at the 7th OSI level which is the application layer and ontop of `UDP/53`. DNS went under a lot of transformation over the year since it was first introduced at 1983, today it exists in many variants that each serve a different purpose or build on previous implementation of the protocol. Some of these are:

> #### `DNS over TCP` (TCP/53)
>
> Provides more stability, longer answers and re-use of long-lived connections between clients and servers.

> #### `DNS over TLS` (TCP/853)
>
> Provides encryption to the entire DNS session, requires a dedicated DoT server.

> #### `DNS over HTTPS` (TCP/443)
>
> Tunnels DNS over HTTPS.

> #### `DNS over QUIC` (UDP/853)
>
> Uses the transport protocol QUIC to transport messages, provides a lot of benefits that TCP does with encryption and speed.

> #### `DNS over Your Mother` (UDP/69)
>
> Tunnels the requests thrugh your mom, however some responses may be lost in the way back due to the fact that your mom ate them.

And there are more. 


### WHY WAS IT INVENTED
Computers can not communicate with domain names alone, computers needs Ip addresses to be able to do that. But humans can't remember IP addresses very well, unlike Domain names. Imagine that if you wanted to visit the youtube homepage on your browser you'd have to type youtube's raw ip, It would be annoying and hard to remember, additionaly you'd have to do the same for every other site you want to visit which is practically impossible and absolutely uncomfortable for the end user.

One of the most common use case of DNS is resolving URLs to ip addresses, But not only. DNS doesn't only deal with URLs, it can actually translate the name of any computer that is registered into a `DNS server` but we will get to that later. And DNS can also resolve ip addresses into Domain Names the other way around.

### HOW DOES IT WORK

#### DNS Tables and Cache
The first step of the translation actually happens on the host, the host will try to resolve the domain name by looking in their DNS cache and Hosts file. If they can't find the site name in there then they will send a DNS request to their **assigned DNS Server**. DNS works on a request response basis, meaning I can **send a server a request (DNS Query)** and **it will give me a response (DNS Response)**. When the server recieves the request with the ip it will look into it's own `DNS Table` to try to resolve the address.

A `DNS Table` is a long list of **DNS records**, basically a list of the Domain name to IP solution **it knows**. A DNS record can be of several types but we will focus on the main ones, '`A`' and '`PRT`'. 'A' is the type that defines Domain Name to Address resolution, this record will be used to translate a given domain name to it's corresponding ip ('A' for IPv4, 'AAAA' for IPv6). A 'PTR' type record will do the opposite and resolve the address into a domain name. DNS can also resolve email addresses to route outgoing emails to an SMTP email server using the `MX` type and for other services using the `SRV` type.

#### DNS Query Types
There are multiple types of querys that the host can ask of the server to perform in order to resolve the address.

> #### `Recursive Query`
>
> In a recursive query the DNS server tries to find the **authoratative** DNS server of the address (The DNS server containing the resolution), through a recursive process starting at the Root DNS server. The server *must* return a response, either the resolution or an error.

> #### `Non-Recursive Query`
>
> In this query either the DNS server has to have the dns resolution or it queries the authoratative server for that hostname (which it must be familiar with through an NS type record).

> #### `Iterative Query`
>
> The iterative query will try to return the best answer from it's cache and if it cant it will redirect the client to the DNS Root Server or the Authoratative server for that resolution where the client will have to repeat the query.

#### Getting a response
Finally, the server will return a reponse to the client which it will use. Either the server gives the client the resolution IP or it will error, in case the server does resolve the address the Client will `keep that resolution in it's own DNS Cache` to make future resolution of that name instantaneous.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Link-Local Multicast Name Resolution A.K.A LLMNR
###### *[#Layer-7] [#UDP/5355]*

LLMNR is used to **enable name resolutions** in places **where dns fails** to do so. LLMNR supports all DNS formats, types, and classes, while operating on a separate port from DNS (listening to queries on UDP/5355), and with a separate cache. LLMNR only operates on the local subnet, making it unviable as a DNS substitute.

### WHY WAS IT INVENTED
LLMNR provides name resolution ability inside the local-link (physical network) when dns is unavialable/unreliable or when it fails to return an answer. It was introduced as an IPv6 work around to NBT-NS which only supports IPv4.

### HOW DOES IT WORK

> #### `LLMNR Query`
>
> A host will look in it's LLMNR cache, if it doesn't find the address it will send an LLMNR Query for it with a specified type (Like DNS) to the Multicast address matching the underlying Layer 3 protocol.

> #### `LLMNR Responders recieve the request`
>
> Any host configured as an LLMNR responder in the local link gets the query and checks their LLMNR records, they return a response only if they are the **authorative responder** to the address queried. The response is sent as a **unicast to the query source address**. 

> #### `Sending Host Processes The Response`
>
> The host that send the query processes the response and adds the resolution to their LLMNR cache.


Do note that **LLMNR should be disabled** in your local link network through GPO or other configuration due to it's critical security flaws.



###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Dynamic Host Configuration Protocol A.K.A DHCP
###### *[#Layer-3] [#Portless]*

DHCP is used to assigned unique IP addresses to hosts in the network automatically, it can also assign other network configuration like subnet mask, DNS server and Default gateway. DHCP can provide a client with all the parameters it needs in order to operate in the network.

### WHY WAS IT INVENTED
Originally, when setting up a new netowrk or adding a new computer to an existing network the IP address, Subnet Mask, Default Gateway and DNS Server Address needed to be put manually into the new machine's network configuration (ncpa.cpl). This kind of IP assignment is known as `Static IP`. However that is uncomfortable, messy and hard to manage. DHCP solves this issue by assigning machine `Dynamic IP Leases` and configuring all the essentials automatically.

### HOW DOES IT WORK
DHCP works on Server-Client framework, dhcp server services can be run on host machines but also on routers.
The DHCP addresses that are given to the machines are merely `leased`, meaning they will expire after a given time and the client will have to renew their lease. That is done in order to retrieve IP addresses of disconnected machines since the server cannot know when or if they were disconnected and no longer occupy the address.

DHCP Servers offer up IP addresses based on a configured `DHCP Scope` on the DHCP Server. A DHCP Scope is a range dictated by a **Start and End Address and subnet mask**. It will look into the subnet that the client is located at and at the available IP addresses and offer one of those. DHCP Scopes have to be manually configured.

Machines on the subnet may also reserve addresses on the DHCP Server in order to always recieve the same address, the reservation is made using the MAC address.

### IP Assignment Process
> #### `DHCPDISCOVER`
> 
> When a client detects that it has no IP (And DHCP is enabled on it) it will look for a DHCP server on the network using a broadcast message. The DHCPDISCOVER message MAY include options that suggest values for the network address and lease duration.

> #### `DHCPOFFER`
> 
> Each DHCP Server that the message gets to may reespond with an available IP address to the client in DHCPOffer message.

> #### `DHCPREQUEST` 
> 
> The client can send a DHCPREQUEST message to a server in order to:
> * Accept/Decline a DHCPOFFER, a client will accept one offer and deny all other.
> * Request a extension to an existing lease.

> #### `DHCPACK`
> 
> Server sends network configuration parameters, including committed network address.

at the end, when a computer wants to disconnect from the network it can send.
> #### `DHCPRELEASE`
> 
> Client to server relinquishing network address and cancelling remaining lease.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Network Basic Input/Output System A.K.A NetBIOS
###### *[\#Layer-5]*
 
The Network BIOS Protocol is a protocols that was used as a standard for communicating messages between two endpoints in the Link-Local Network. It provides `basic functionality for communication`. 

### WHY WAS IT INVENTED?

NetBIOS was created in order to free application from the need to understand their network and implement a lot of networking specific features that would let them run as needed. Basically it was a `standard to lay groundwork for communication` that would be easy and independant.

### HOW DOES IT WORK?

NetBIOS lets application that rely on it to send messages over the **local area network**. It is **not** a networking protocol, as it does not have a datagram format of it's own. Over NETBIOS, application can form sessions with eachother, communicate connectionless and even resolve NetBIOS names.

To falicitate those network functions NetBIOS seperates to 3 services:

> #### `NetBIOS Name Service`
> ###### *[#UDP/137]*
> 
> NetBIOS provides it's own type of network architechture where each computer is represented by a 16 byte `name`, 15 ASCII characters to represent the computer and the last as a bit-field that represents what type of services the machine provides. The name will then be registered to an ip address (Only IPv4), will will enable name resolution.
> 
> NBNS (NetBios Name Service) also acts as an "Alternative" method of name resolution incase DNS and LLMNR both fail, The names are resolved by WINS (Windows Internet Naming Service) Servers. Where, instead of Domain names the server resolves NB names to IP addresses. The Servers are used to balance networking loads and to avoid flooding the network with broadcasts.
> 
> NOTICE: NBT Names are apply only to the link-local network, they are not to be confused with domain names.

> #### `NetBIOS Session Service`
> ###### *[\#TCP/139]*
> 
> Applications using NetBIOS can establish a NetBIOS over TCP (NBT) session with a "call" command, then communicate with "send" and "recieve" commands. Since NBSS uses TCP as it's base it allows for bigger messages, transmission control and loss recovery.

> #### `NetBIOS Datagram Service`
> ###### *[\#UDP/138]*
> 
> Applications may also communicate over NetBIOS connectionless, using individual datagrams over the NetBIOS Datagram Service. The datgram services allows unicast, multicast and broadcast datagram messaging, using "group ips", an application can listen on a designated group ip to recieve a satagram sent to that group's members.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Teletype Network A.K.A Telnet
###### *[#Layer-5] [#TCP/23]*

Telnet is a session layer protocol that enables terminal access to an application on another host. Allowing the user to send commands to another endpoint rmotely.

### WHY WAS IT INVENTED?

The idea for a terminal that can interface with the network was proposed on [rfc 15](www.rfc-editor.org/rfc/rfc15.html "Network Subsystem for Time Sharing Hosts") back in 1969. The documents discusses the idea of remotely accessing another machines terminal abstractly and even having the ability to transfer files over the network. The host machine would just send each chacrater input to the remote machine, and the host machien would have a special format to ented basic commands.

### HOW DOES IT WORK?

The telnet protocol is mostly used by the telnet subsystem in your machine, using that system you may connect to another machine with the protocol and send information (namely commands or their outputs) bi-directionaly using the command line interface. Telnet is strictly characterial, and work over platform as the subsystem can decode characters to match the system requirement.

Since at the time of the protocol's manifestation the cyber threat wasn't as potent, all communication over TelNet is sent as cleartext (Not encrypted). Given that, telnet has to be manually enabled with the newer systems nowadays and they are frowned upon.

However, telnet still has uses like:
* Old Systems that don't support newer protocols like SSH.
* Configuring routers and network devices.
* Checking if ports are open.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## File Transfer Protocol A.K.A FTP
###### *[\#Layer-7] [\#TCP/21] [\#TCP/20] [\#TCP/Highports]*

The file transfer protocol is used to *transfer files* from one host to another on the wide net.

### WHY WAS IT INVENTED?
The protocol to transfer files over the net was first prposed in 1971 on [rfc 114](www.rfc-editor.org/rfc/rfc114), the document discusses the usability of a protocol to act as an abstraction layer when a user from one machine wants to retrieve a file from another machine to his own while not being familiar with the other machine's file system and interactive commands (unlike Telnet). Basically, the protocol is supposed to act as a middle man for transferring files quickly and efficiently between one host and another that knows both host's languages and provides it's own commands. The protocol also allows remote storage of files.


### HOW DOES IT WORK?
FTP operates as a kind of abstraction between the user typing service specific, both hosts using the service will have a `protocol interperter (PI)` of either a cient or server type and a `protocol data transfer process (DTP)`.

The service allows the user to type in commands on the service specific terminal application after connecting to the server. The commands are global to all machine platforms, and are not OS dependant. Like: `mkdir` to create a directory, `bye` to disconnect from the server ,`help` that calls small Noam for help, `USER` to send username and `PASS` to send password in order to authenticate.

> #### `Commands`
>
> Using a terminal the user can send and extract fils from the conneted host. The commands and files are transferred on two different data streams.
Firstly, the `PI` will initiate a `control connection` with the other endpoint on the configured listening port (defaultly 21). On that connetion it will send commands and recieve output, that way the file transmission will not hinder command operations.
> 
> BIG DISCLAIMER
> 
> Just like the TelNet, at the time of this protocol's creation the cyber threat wasn't as prevalent. Due to this, commands and file data are sent over as **cleartext**!!!
> 
> So like, don't use it :D use FTPS instead ;) xoxo.

### FILE TRANSFER

> #### `Interaction`
>
> Once the user uses one of the file transfer commands the `DTP` will initiate another `data connection` on which it will send the file.
>
> There are two methods in which it may connect:
> * Active - The server will initiate the connection to the user's host from port 20.
> * Passive - If there is a FW in the network that blocks incoming connections the user's host will be the one to initiate the connection on an agreed high port.

> #### `Transfer Modes`
>
> FTP supports multiple types of transfer modes:
> * ASCII   - Translates the file to an ASCII char set that is known to the FTP service back and forth.
> * EBCDIC  - Uses a differnt encoding. if one endpoint in the connecton uses it, it is forced. 
> * Image   - For transferring binary files, does not encode.

> #### `Transmission Modes`
>
> FTP transmitting the data from one endpoint to another hapens over the data connection using different methods:
> * Stream      - The file data is sent as a stream of bytes.
> * Blocking    - The file data is dissected into blocks and sent 1 by one. At the beginning of the block `3 header bits` are added to catalouge the block.
> * Compressing - For really big files there is the option to operate a "run-length encoding" function on the data to reduce it's size. Though this is pretty useless as compression happens anyways in other aspects of networking and file system management.

> The FTP protocol acts as a ftp-specific terminal on the remote machine, giving the user the ability to input commands to afect the remote machine. The main goal of ftp is to manage remote data and extract it.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------

## Trivial File Transfer Protocol A.K.A TFTP
###### *[\#Layer-7] [\#UDP/69]*

Trivial File Transfer Protocol is a fork of ftp that provides it's services on the **local-link network**. It was designed as a **lighweight** ftp version to transfer configuration files to network devices in the link-local network.

There are several differences between `FTP` and `TFTP`:

> #### `Authentication`
>
> Ftp requires authentiation while tftp doesn't.
>
> Big danger!!! >:D

> #### `Transportation`
>
> Ftp uses tcp as it's transportation protocol, tftp uses udp.

> #### `Use`
>
> Ftp is used to transfer files and run commands over connected endpoints, tftp is mainly used to transfer configuration files on the LAN.

### HOW DOES IT WORK?

This protocol too works on a `Client/Server` basis, note that some concepts and terms carry over from the FTP section.

> #### `DOWNLOAD`
>
> The way that the client sends commands to the server stays the same. The server listens for connections on UDP/69 (haha :D) and the client can  initiate a connection on it from a high port and transmits commands over it. Then once recieving a command to download a file, the server will open up another tftp process and connects back to the client high port.
>
> The way file transmission works is the server `breaks the needed file` into a `series of blocks` and send them to the client one by one `on UDP`. The server will then listen for an `acknowledgment` that the file block was recieved and send the next block in the series.

> #### `UPLOAD`
>
> When the server recieves a commands to upload a file from the client it will `open a new tftp process` and `bind it to a random high port`. The new data transfer process will connect to the command sending port from the client and send an `Aknowledgment` to notify of the open port.
>
> From then on it's like uploading a file. The `client will slice the file` into a series of blocks, and send them one by one on UDP to the new server port. After sending each block it will `wait for an acknowledgment` before sending the next block.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## HyperText Transfer Protocol A.K.A HTTP
###### *[#Layer-7] [#TCP/80]*

HTTP is the protocol used to send web pages over the internet. This is a very broad definition of the protocol but we will get into it later (Like I did to your mom).

### OVERVIEW

Http evolved so much over the years so I'll ty to summarize it to as little as possible.

Overall, HTTP works as a request response type protocol, just like dns and dhcp. The whole point of it is to get the hosted webpage from the server, like "www.google.com" where I research this stuff or "jokes.moomoo.co.il" where I get my your mom jokes.

[In the beninging](www.youtube.com/watch?v=fWKB8zdVM-U) the internet was in it's diapers, all you could find are super simple webpages like blogs. There was no need for a complicated protocol to transfer this data and that's where HTTP came in.
![The Original Spacejam Website](www.themarysue.com/wp-content/uploads/2010/12/SpaceJam_Home.png)
###### pretty simple right?


> #### `HTTP/0.9`
> 
> It was used to send the HTML code that built the site as plaintext to the client visiting the site. **Emphasis on cleartext**.
> On this protocol you only used mentioned a method (GET) and a resource in your request.

> #### `HTTP/1.0`
>
> This version of HTTP added a `header`, in the header was a lot of information regarding the session and request. Most importantly the method and resource.
> 
> It added two more methods, POST and HEAD that allowed the user to send data to the site, and Head that returns the header information (metadata).
>
> It added content types, so that from now on you could send more types of content and not only HTML. Like your mother, except the fact she wont fit on the bandwith :(

> #### `HTTP/1.1`
>
> This version was introduced only a year after the last one and it built upon it. We're talking:
> * **Host header** - to identify servers that have multiple Domain Names.
> * **Persistent connections** - HTTP now keeps it's connection alive after sending a request and doesn't open a new session for each request (Which takes a full RTT thanks to the TCP handshake.. Ugh, digustang).
> * **6 New methods** that were really helpful in the usage of the protocol. (PUT, PATCH, DELETE, CONNECT, TRACE, OPTIONS)
>
> Though the protocol **still communicated over cleartext**, which wasn't very cool.

> #### `The latest version: HTTP/2.0`
>
> This protocol ought to make things a whole lot better:
> * **Multiplexing** - Now you can send multiple requests on multiple concurrent connections that represent a single session using multiple TCP connections!
> * **Prioritizing** - Sending more than one request? Tell what you want to receive first!
> * **Automatic Compression** - Automatic [Gzip](10web.io/site-speed-glossary/gzip-compression/) compression.
> * **Connection reset** - The option to terminate and reestablish a connection with the server, if for some reason you need it.
> * **Server Push** - Let the server give you all the commonly requested data before you even ask for it, this let's it load balance.

### HOW DOES IT WORK?

Let's break it down one by one, start with the most simple.

> #### `Initializing a Connection`
>
> Since HTTP uses TCP for transport, and HTTP has no "handshake" of it's own all that's needed is the TCP 3-way-handshake.
>
> After initialization the clinet and server may exchange `FRAME`s, units of http data.

> #### `Methods and Responses`
>
> Http works as a `Master-Slave` or `Request-Response`. The client sends a `DATA Frame` that includes a `method` to the server and the server returns a response. Like, the client makes a request for a webpage and the server can return the webpage if it has it, or return an error if something went wrong.
>
> The method for requesting content off of an HTTP is called `GET`. When a client sends a GET request the server will look for that resource in it's directory and send it over, most of the time encoded.
> This method constitutes most of the protocol's use, when loading a website on your browser your machine makes a GET request for all of the resources that need to be displayed, those can be:
> * ALL of the images, videos and gifs.
> * The website's HTML code.
> * The website's JS scripts, CSS code.
>
> When a resource if found the server will return a "200 OKAY" response, that means that the request was processed and replied to correctly. Otherwise it may return the infamous "404" error, but we will get to that later.

> #### `Methods and Response Codes`
>
> Other than `GET`, HTTP offers many more commands to interact with the server, here are a few.
>
> * `HEAD`      recieve an exact response like `GET` but without any resource data.
> * `POST`      "Submit" an entry to a server side resource, like a database. Most of the time this will make a change in that resource.
> * `PUT`       Create or replace an existing resource on the server side.
> * `DELETE`    Delete a resource on the server.
> * `CONNECT`   Used to tunnel a connection to an http server through a VPN.
>
> And there are a few more.
>
> Reponse codes are numerical numbers that represent the response that happened on the server, if the processing of the request was handled fine then the code will be **200**, if a resource was not found then the code can be **404**. Response codes are broken down into 5 families, dictated by the first digit (the hundreds digit):
>
> | CODE      | FAMILY                            |
> | :-------: | :-------------------------------- |
> | 100 - 199 | Information responses.            |
> | 200 - 299 | Successful responses.             |
> | 300 - 399 | Redirection responses             |
> | 400 - 499 | Client Error responses.           |
> | 500 - 599 | Server Error responses.           |
>
> For example, 200 will be the first code in the `Successful response` family and it stands for "The request succeeded". 201 stands for "The request succeeded and a new resource was created.", this may be in response to a `PUT` command.

> #### `Multiplexing Connections`
>
> Using HTTP/2.0 a client can send multiple `Frames` simultaneously using a concept called `STREAMs`. A `stream` is a bi-directional flow of data betwen the data and the server, using a `stream` the client sends and receives data from the server.
>
> Basically all communications over the networks happen on streams, what's special about HTTP/2.0 is that it allows opening and managing multiple streams under one session. Then the protocol can send request over different streams and not be bottlenecked by a big resource.
> 
> * A stream **can** be opened and closed by either client or server
> * A stream **can** be uni-directional or bi-directional.
> * A stream **can** be given a priority.
> * A stream **has** an identification number.
> * A stream **can** be dependant on another stream.
> 
> HTTP provides many `FRAME`s for managing streams.
>
> * `DATA`          This is the frame that will contain http request and response data. If the **END_STREAM** flag was set in the frame, then this frame will be the last DATA frame to be sent on the current stream.
> * `HEADER`        This frame is sent in order to `create a new stream`, inside the frame will be parameters like: Stream identifier, dependencies and priority.
> * `PRIORITY`      This frame is used to change the priority of the stream.
> * `PUSH_PROMISE`  This frame tells the peer that you want to create a stream/streams.

###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Simple Mail Transfer Protocol A.K.A SMTP
###### *[#Layer-7] [#TCP/25]*

The Simple mail ransfer Protocol's job is to **mail and/or send** data to another `smtp server` on the network, similar to [Telnet](#teletype-network-aka-telnet). Smtp is used only for message forwarding, not retrieving.

### WHY WAS IT INVENTED?
The smtp protocol was invented in order to provide the ability to send mail over the network.

### HOW DOES IT WORK?
SMTP is used to relay mail messages over the network from the local network's smtp server to the recipient local network's smtp server. More accurately the stmp protocol is used as a guideline to transfer mails from a **client MTA** (**M**essage **T**ransfer **A**gent) to a **server MTA**. Please do notice that the smtp protocol usually acts as a background service for e-mail applications on a user's computer like Office 356.

> #### `Initializing a connection`
>
> An smtp session is initialized when a client establishes a connection with the server. A server will usually listen on the well-known port TCP/25.
> 
> 1. The server then send an `opening message`, something like "Yo, you wanna talk?".
> 2. The client send back an EHLO or HELO response, EHLO is like "Hello I am <client> and I support more calls" and HELO is like "Hello I am <client>".
>
> After this the connection is established and the client can begin sending commands to the server.

> #### `SENDING`
>
> Before we can begin sending e-mails we need to set some things straight. Like, **who are we?**, **Who do we want to send to?** and **what am I sending?**. To answer the first question is the `MAIL command`, the MAIL command tells the server where the mail came from, specifically a mailbox. Then comes the questin of who am I sending to? and that is answered with the `RCPT command` which tells the server what the mailbox I am aiming for is. The server may error or one of these commands, but if not it will return a `250 OKAY` reply to both of them.
>
> After configure sender and recipient we need to input the mail contents, we will use the `DATA command`. We will them input the lines of ASCII text we want to send and end it with a "." line, everything between the line containing the dot and the DATA command will be seen as a part of the e-mail.
>
> If the DATA command processes correctly, the server will return a `250 OKAY` response and send the email to the **recipient's network's Server MTA**.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Post Office Protocol 3 A.K.A POP3
###### *[#Layer-7] [TCP/110]*

POP is used to let the user access their mails online, since smtp is only able to push emails to a destination mailbox.

### WHY WAS IT INVENTED?
Smtp is the protocol used to send and forward mails to a mailbow on the mail server, but to view those emails interfacing with the server directly is required. No only do you need to go on the server but you also need both yout machine and the server to be connected. POP solves that issue by downloading you emails from the mail server for you, so you can access them from your machine and view them offline.

POP3 is the latest version of the protocol with POP 1 being the first, over the years more features have been added to the protocol to make it more useful.

### HOW DOES IT WORK?

> #### `INITIATING A SESSION`
> 
> The server will listen to on a port, once the client connects and preforms a tcp-handshake it will send a greeting message and wait for commands, at this moment a session is established. During it's lifetime a session goes through multiple states..

> #### `AUTHENTICATION`
>
> The client has to authenticate to the server using USER and PASS commands to pass their credentials. After authenticating correctly the server opens the client's mailbox and locks any access to it's content so that they don't change in the midst of a session.
> 
> **NOTICE** communication in POP is not secure and transmitted as cleartext. Special applications of this protocol do provide encryption though.

> #### `[POP 1] TRANSACTION + UPDATE`
>
> Now the user can extract the information from the server to their local machines in one of two ways:
>
>   * Download all e-mails and delete them from server. (`RDEL`)
>   * Download all e-mails but keep them on the server. (`RETR`)
>
> After choosing an option the client can command to be sent all the read information (`RCEV`) and after recieving all the mails they send a confirmation that they recieved them (`RCVD`)

> #### `[POP 3] TRANSACTION`
>
> Now the user can interact with their mailbox and it's content, the user can:
>
>   * `LIST`    all mails.
>   * `STAT`    get information about the amilbox in the server.
>   * `RETR n`  retrieve the `n`th mail from the mailbox.
>   * `DELE n`  mark the `n`th mail in the mailbox as deleted.
>   * `QUIT`    enter the UPDATE state.
>
> Using these commands the user can interface with their mailbox from their local machine, of-course applications that use POP3 can represent those commands in super awesome ways. Like list all the emails in a stylized list on the gui.

> #### `[POP 3] UPDATE`
>
> In the UPDATE state the server deletes every mail marked as deleted by the user, then it releases the access lock it has put in the AUTHENTICATION state and disconnects the TCP connection.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Internet Message Access Protocol A.K.A IMAP
###### *[#Layer-7] [#TCP/143] [#TCP/993] [[#What_Came_In_The_Mail?](youtu.be/CjgXx6QyZRg?t=10)]*

IMAP also dabbles in reading mail messages from a server, but it is way more efficient than POP. It was conceptualized becayse POP had many problems and IMAP was meant to address them.

### WHAT DOES IT DO?

IMAP allows the remote managment of folders called mailboxes on the mail server. IMAP allows more delicate control and syncronization between devices, it also allows to connect more than one client to the same mailbox at a time and more. The model of the protocol is also Server and Client to retrieve mail from the server and manage it remotely.

### HOW DOES IT WORK?

 > #### `Initializing a session`
 > 
 > The server will listen on both ports 143 for cleartext communication and 993 to implicitly initiate a session over tls. Once connecting to one of those the server will send a greeting response and wait for commands, at this point a session was created. A session can inhibit one of four states, a session may begin as one of the next three states.

> #### `STATE 1: Not Authenticated`
>
> In this state the client will have to preform a login operation in order to enter their mailbox.
>
> If the session is initialized on a cleartext port a client may send the `STARTTLS` command to begin session encryption.

> #### `STATE 2: Authorized Connection`
>
> If the session was opened pre-authenticated, meaning that the client gave valid credentials in the session initialization.
> * A client can choose the mailbox they wish to interacrt with, with the `SELECT` command.
> * A client may open a mailbox identically to the SELECT command but in read-only mode, with the `EXAMINE` command. 
>
> A client can perform more operations in this mode like create, delete or remane a mailbox.
> 
> but I will not go over it.

> #### `STATE 4: Logout State`
>
> If the session initialization failed or the user input a `LOGOUT` command the server and client will terminate their connection.

> #### `STATE 3: Selected State`
>
> In this state the user has selected a mailbox and may interact with their mail messages.
>
> A client may:
> * `UNSELECT` the mailbox.
> * `SEARCH` for a message with given information.
> * `MOVE` or `COPY` a message from their mailbox to another.
> * `EXPUNGE`, meaning delete all messages flagged with `/delete` 

Here is a flow-chart of how moving between states works.

```
                   .----------------------.                     LEGEND
                   |connection established|                     (1) connection without pre-authentication
                   '----------------------'                     (2) pre-authenticated connection
                              |                                 (3) rejected connection
                              V                                 (4) successful authentication via command
            .--------------------------------------.            (5) successful SELECT or EXAMINE command
            |          server greeting             |            (6) CLOSE or UNSELECT command, unsolicited CLOSED response code, or failed SELECT or EXAMINE command
            '--------------------------------------'            (7) LOGOUT command, server shutdown, or connection closed
                      |  (1)       |  (2)        |  (3)
                      V            |             | 
            .-----------------.    |             | 
            |Not Authenticated|    |             | 
            '-----------------'    |             | 
             |  (7)   |  (4)       |             | 
             |        V            V             | 
             |      .----------------.           | 
             |      | Authenticated  |<-.        | 
             |      '----------------'  |        | 
             |        |  (7)   |  (5)   |  (6)   | 
             |        |        V        |        | 
             |        |     .--------.  |        | 
             |        |     |Selected|--'        | 
             |        |     '--------'           | 
             |        |        |  (7)            | 
             V        V        V                 V 
            .--------------------------------------.
            |               Logout                 |
            '--------------------------------------'
                              | 
                              V 
                .-------------------------------.
                |both sides close the connection|
                '-------------------------------'

Flowchart taken from official [IMAP version 4 revision 2] rfc 9051.
```

### MESSAGES

A message is an item inside a mailbox that was sent from another mailbox, unltimately the point of the protocol is to let the client read these messages remotely.
A message may be flagged as one of these:
  *  `\Seen`, Message has been read. (For example: Marked as read in Gmail)
  *  `\Answered`, The message has been sent a reply.
  *  `\Flagged`, The message is urgent/special. (For example: Important mails in Gmail)
  *  `\Deleted`, The message will be deleted when an `EXPUNGE` command will be executed.
  *  `\Draft`, The message in not complete.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Server Message Blocking Protocol A.K.A SMB
###### *[#Layer-7] [#TCP/445] [#TCP/139]*

SMB is a protocol meant for file sharing, not to be confused with file transfer. This protocol allows a user on a Windows machine to share a folder over the network and let other user iteract with it on their local machine without downloading it. It works on a client/server model and uses commands to make the other endpoint operate.

### WHY WAS IT INVENTED?
The protocol was invented on 1983 by Barry A. Feigenbaum at IBM in order to provide Hosts, printers and other network nodes `shared access` to files on the network. The protocol was then picked up by Microsoft and even since then it was buit upon with the 4th version of the protocol being the current standard. The protocol was also imitated and ported over to Linux to provide Unix based systems access to file sharing and interfacing abilities with Windows machines.

Over the years the protocol has been divided into dialects (or versions), with each of them building ontop of the one prior to it or changing something. Here are just a few:

> #### `SMB V1.0`
>
> This was the first SMB protocol, it wasn't very efficient or secure. It is widely known that you should not use this protocol.

> #### `SMB V2.0`
>
> This was the second verision of the protocol, created 20 years after the 1st one. Released with Windows Vista this protocol solves a lot of issues that the last version had like:
> * Wayyy less commands.
> * Better performance.
> * Better security.

> #### `SMB V3.0`
>
> This version was introduced with Windows 8 and Windows Server 2012, it provides a lot more facilitation abilities and security solutions. For example message signing and better, strong encryption.

### HOW DOES IT WORK?
Communication with the protocol starts when the client send a request and the server returns a reponse, initiating a connection. Then a session is created with a set of keys, one for encryption and one for signing messages, known to both sides.

As it's transport layer used can be either TCP (on port 445) or NetBIOS over TCP (nbss on port 139), The protocol also requires authentication. Either using NTLM or Kerberos (for domain purposes), the first one being vulnurable to attacks. After the authentication and checking permission for the shared folder the server will open a connection to the folder where thec leint can perform various operations, like read and write information from/to it, rename it, delete it, create a new file, copy and save it and more. All of these operations are done on the server side using commands sent by the client.

At the end the client will send a command to close the connection and session and the server will follow through, the affect of the operations that the cleint made are saved real-time. Also high versio of SMB allow file handles that can persist though unexpected disconnections.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Simple Network Management Protocol A.K.A SNMP
###### *[#Layer-7] [#UDP/161]*

The Simple Network Management Protocol is used to collect device information, like cpu usage, up status and bandwith from all the devices over the network to allow simple network managment from one machine very easily.

### WHY WAS IT INVENTED?
The protocol was invented back in 1988, back then networks weren't as big as they are today but they were getting bigger. This is why there was a need for a way to manage IP networks easily, a need which was fullfilled by this protocol.

### HOW DOES IT WORK?

> #### `MODEL`
> 
> This protocol uses a Controller-Agent model, and it operates on the UDP transport protocol. As to how it get's the information is pretty simple, every device on the network has a table called a `MIB` (Management Information Base). In the MIB will be a list of `Object identifiers`, `Objects` are like properties of the network device, an Object can be the name of the device or a routing table of a router.

> #### `Server & Requests`
>
> The controlling server, called `NMS` (Network Management System) can then fetch the information from the MIBs using `GET requests` (Yes, like http you nerd). The NMS may also change the values of the objects on the agent's machine using `SET requests`.

> #### `TRAP/INFORM messages`
>
> If an interface goes offline on a specific device containing an snmp agent, the agent will send either a `TRAP` or `INFORM` alert to the NMS to notify it. Both alerts server the same purpose but are still a little different, that is in the way that they are communicated. An `INFORM alert` will wait for acknowledgment from the server and will resend if it didn't get a message back saying it was acknowledged, a `TRAP alert` will send a forget.

So basically using these operations and with a MIB table on all devices a network administrator can display all the information about their network's devices, and get notified when a device doesn't work as needed.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Network Time Protocol A.K.A NTP
###### *[#Layer-7] [#UDP/123]*

The protocol is used to synchronize time over all the machines in the network, time synchronization is crucial when it comes to many things in a network like logging events. in order to do that an NTP server will dictate the time for all the machines in the network. 

### HOW DOES IT WORK?

The way time synchronization works over the internet is by devices syncing their internal clocks with eachother, the reliability of the device's clock sncronization is called `stratum`. `Stratum` is measured from 0 - 15, devices that classify as `stratum 0` are usually professional grade time measurement devices like `Atom clocks` and such, those will connect to `Time servers` which will become `stratum 1` since they sync off of a `stratum 0` device.

NTP Servers on small networks will usually syncronize using stratum 1,2 or 3 servers off the internet and then the other devices on te network will syncronize off of that NTP server.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Transport Layer Security **/** Secure Socket Layer A.K.A TLS/SSL
###### *[#Layer-5] [#Application-Based-Port] [#Encrypted]*

Both TLS and SSL are protocols used to encrypt information on the wire in a way that both endpoints can decrypt them, meaning that `they generate a set of symetric encryption eys for the given session`. That is why they belong in the `Session Layer`, the one that is also responsible for encryption.

Nowadays TLS is the standard for session encryption and regarded as the better protocol, originally TLS was born from SSL 3.0 and superseeded it.

```
SSL 1.0 -> SSL 2.0 -> SSL 3.0 -> TLS 1.0 -> TLS 1.1 -> TLS 1.2 -> TLS 1.3
```

### WHY WERE THEY INVENTED?

At the time, most communication through the network happened as cleartext, there was a serious threat from middlemen existing in the network reading all the information. Especially when it came to businesses and online transactions. So in the year 1995 [Netscape](www.youtube.com/watch?v=dQw4w9WgXcQ) introduced this protocol to serve as a porm of packet protection by encrypting packet contents on the wire.

### HOW DO THEY WORK?

The end goal of both protocols (SSL & TLS) is to create ecryption keys to symetrically encrypt the connection. The way both protocol perform this is with a `Protocol Handshake`, then they encrypt the connection using an `encryption aglorithm` with the *exchanged* encryption keys.

But before we start going into that, we need to explain a few things:

> The first thing we need to understand about encryption is the existence of public and private asymmetric-encryption keys, keys that allow encryption with one and decryption with the other.

> Second, everyone can know the other machine's public keys but not private keys.

> Third, we can assume that there is always someone sniffing on the network (a middle man).

*So why are public/private keys not enough?* to keep it short, the middleman can easily switch the server's public key with his. We have no way to verify the integrity of a public key alone.

That is one problem. There are many more:
* `protocol negotiation`, which version of tls or ssl does the machine support?
* `What encryption elgorithm to use`? 
* How to `exchange secret keys`?

The SSL & TLS protocols are made up of 4 sub-protocols, 3 in a higher level and 1 in the lower level. They all work together coherently in order to provide the encryption service that is wanted. Each of the sub-protocols is responsible for something in the functionality of the SSL & TLS protocols:

```
Handshake Protocol       -.                      Establish security parameters and session.              
Change Cipher Protocol*  -|- HIGHER LAYER    Responsible for cipher changing negotiations.
Alert Protocol           -'                      Responsible for alerting if there is an error/warning.
Record Protocol          -- LOWER LAYER     This is the protocol that encrypts, compresses and ecapsulates the application data.

* Doesn't exist in TLS 1.3
```

SSL/TLS also has **two major concepts**:

> `SESSION`
>
> A session is a set of negotiated security parameters (key exchnage & encryption algorithms, SID etc.) between one endpoint and another.

> `CONNECTION`
>
> A connection is the **application of a session's parameters to an active connection**, meaning **every** `connection` has to be tied to only **one** `session`. But, **one** `session` can be **used by multiple different** `connections`.
>
> A connection has two cipher spesification states too, `pending` and `current`. Whenever negotiating parameters, those parameters get put in the pending state. And when the connection receives the relevant message it will change the `current` CipherSpecs using the `pending` ones. 

### Handshake Protocol (SSL 3.0 - TLS 1.2)

> #### `STEP 1: CLIENT HELLO`
>
> Basically the client send a tls/ssl session initiation message, In this message will be a few things that are needed in order to synchronize the encryption. The `latest tls/ssl version the client supports`, `A random number`, `A session identification number`, `A cipher suite` (A list of the **encryption and key exchange** algorithms it supports sorted by preference), `A list of compression algorithms` sorted by preference as well.
> After sending the client hello the client waits for a **SERVER HELLO** message, meaning the server wishes to initiate a connection.
>
> If the server doesn't accept the **CLIENT HELLO** message for some reason then it will send back a `handshake failure` message.

> #### `STEP 2.1: SERVER HELLO`
>
> The server looks at the **CLIENT HELLO** message and chooses all the best compatible options and send them back to the client as a sort of "I choose this from what you gave me". To this, the server will add a random number of his own and dictate the Session identifier.
>
> Depending on the Session ID the server may set already existing security parameters for this `connection` using the session the ID belongs to. Ultimately the server sets the session ID. If the SID is empty the server will create a new session and a new SID for the connection.

> #### `STEP 2.2: SERVER CERTIFICATE`
>
> If the server needs to (which it most probably will nowadays), the server sends it's [certificate](github.com/TimonLevy/Networking/blob/main/Extras.md#what-are-ssltls-certificates) that validates it's authenticity and give the server it's public key.

> #### `STEP 2.3: SERVER HELLO DONE`
>
> This is just an empty message that says "I am done, it's your turn again.". The server may add more messages inbetween the `SERVER CERTIFICATE` message and the `SERVER HELLO DONE` one for special purposes, but sending the `SERVER HELLO DONE` message means that the server is done and not to expect for more.

> #### `STEP 3: CLIENT KEY EXCHANGE`
>
> After the server is done, the client will begin by **initiating a key exchange** that they will later use to **create session keys**. The exchange algorithm is defined by the cipher suite that the server chose in their `SERVER HELLO` message.
>
> We will not go into the different ways to exchange keys, but it is important to note:
> 
> in **TLS** the keys can not be calculated by anyone else other than the client and server since at least one parameter in the formula will be a secret. That is what's called a `pre-master secret`.
>
> In **SSL** a huge exploit was found that allowed attackers to extract the server's private key and possibly find out the key. SSL 3.0 was the first version to support Diffie-Hellman Key exchange, prior to that only RSA was allowed.

> #### `STEP 4.1: CHANGE CIPHER SPECIFICATIONS`
>
> In this step the client sends a `change cipher spec` message telling the server to update it's connection's state.

> #### `STEP 4.2: HANDSHAKE FINISHED`
>
> The client will send a `FINISHED` message that is already encrypted in the negotiated encryption algorithm and uses the exhcnaged keys for encrypting. this message will usually be the last before the encrypted conversation starts between the both but a server can send a `FINISHED` message as well.

```
.               CLIENT                            SERVER
.                 |           CLIENT HELLO          |
.                 | ------------------------------> |
.                 |                                 |
.                 |           SERVER HELLO          |
.                 |        SERVER CERTIFICATE       |
.                 |        SERVER HELLO DONE        |
.                 | <------------------------------ |
Client verifies   | <------------------------------ |
Certificate       | <------------------------------ |
.                 |                                 |
.                 |           KEY EXCHANGE          |
.                 |        CHANGE CIPHER SPEC       |
.                 |             FINISHED            |
.                 | ------------------------------> |
.                 | ------------------------------> |
.                 | ------------ENCRYPTED---------> |
.                 |                                 |
.                 |        CHANGE CIPHER SPEC       |
.                 |             FINISHED            |
.                 | <------------------------------ |
.                 | <-----------ENCRYPTED---------- |
.                 |                                 |
.                 |             APP DATA            |
.                 | ------------ENCRYPTED---------> |
.                 |                                 |
.                 |             RESPONSE            |
.                 | <-----------ENCRYPTED---------- |
.                 |                                 |
.                 V                                 V
``` 
A basic diagram showing the flow of a handshake. there may be more messages then this, but this is the bare-necessity.

### Change Cipher Spec Protocol

This protocol is the simplest of the sub-protocols, it is made up of a single byte with the value of "1". The purpose of this protocol is to tell the recipient to update the connection's state to use the negotiated CipherSpec immediately (Cipher Specifications: keys, Algorithms, etc.). This message will be enccrypted by the current connection's CipherSpecs.

### Handshake Protocol (TLS 1.3)
TLS saw a big revision in the handshake process, one that made it way less viable for attacks, quicker and kept `Perfect Forward Secrecy`.

Here's an overview of it.
```
.               CLIENT                            SERVER
.                 |           CLIENT HELLO          |
.                 | ------------------------------> |
.                 |                                 |
.                 |           SERVER HELLO          |
.                 | <------------------------------ |
.                 |                                 |
.                 |        SERVER CERTIFICATE       |
.                 |             FINISHED            |
.                 | <-----------ENCRYPTED---------- |
.           1 RTT | <-----------ENCRYPTED---------- |
.                 |                                 |
.                 |        FINISHED + APP DATA      |
.                 | ------------ENCRYPTED---------> |
.                 |                                 |
.                 |             RESPONSE            |
.                 | <-----------ENCRYPTED---------- |
.                 |                                 |
.                 V                                 V
```

> #### `STEP 1: CLIENT HELLO`
>
> The client send most of the same things that it used to. `Cipher suites`, `Client Random`, `tls/ssl version` (always set to 1.2), `session id` and added to that is a new attribute `key share`. The protocol specification only allow perfect forward secrecy compatible ciphers, so this `key share` value is the value the server needs in order to generate the key. The client does not know which protocol the server will pick so it may send a few `key share`s to match it's cipher suites.

> #### `STEP 2: SERVER HELLO`
>
> The server will pick a cipher, calculate the `client key` using the client's `key share`. Then it creates a new `key share` of it's own and sends that and the chosen cipher to the client.
> 
> After this message the connection will be encrypted.

After this the server will send a certificate to validate itself and a finished message to let the client know it's done. Using the server's `key share` the client can `generate the server's symmetric key` and decrypt the messages from the server.

* The client will send messages encrypted with it's client key and decrypt messages from the server using the server's key.
* The server will send messages encrypted with it's server key and decrypt messages from the client using the client's key.

Another feature TLS version 1.3 provides, is the ability to send a client hello message together with the first byte of data already encrypted! This is called a 0-RTT handshake.

### Alert Protocol

This protocol will alert the recipient in case of an error or warning, the alerts are labeled by level of severity. In case of a fata severity the connection is immediately terminated and the session nullified, to not let a new connection use the same parameters and potentially repeat the error.

### Record Prtocol

The record protocol is responsible for the encryption of the message, the validation of message integrity, fragmantation and the compression. It does that using the current CipherSpecs which will include the encryption algorithms, keys, compression algorithms and [MAC algorithms](github.com/TimonLevy/Networking/blob/main/Extras.md#message-authentication-codes).

However, TLS uses a different MAC lgorithm called HMAC, essentially it hashes the message to make it more compact but still representative of the original message. 

The whole process goes like this:
1. The mesage will be fragmented.
2. Each fragment will be compressed and become smaller.
3. Each fragment will have a MAC calculated and appended to it, the fragment will be back to it's original size.
4. The fragments will be encrypted and a  Record Protocol header will be added to it.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Hypertext Transfer Protocol Over TLS/SSL A.K.A HTTPS
###### *[#Layer-7] [#TCP/443] [#Encrypted]*

This protocol isn't really a standalone protocol but a more secure version of the [HTTP](#hypertext-transfer-protocol-aka-http) protocol. HTTPS uses tcp port 443 to connect, and HTTP version 2.0 + TLS version 1.3 (curently). The protocol allows encrypted communication over the internet using the TLS/SSL protocol.

for information about HTTPS/2.0 refer to the [HTTP](#hypertext-transfer-protocol-aka-http) section.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Internet Protocol Security A.K.A IPsec
###### *[#Layer-3] [#VPN] [#Encrypted]*

IPsec is a protocol that provides VPN services through encryption, A VPN is "A private connection over the public network". IPsec encrypts all packet layers above layer 3, it does that with a process that is simillar to TLS/SSL. Something resembling a handshake, where all the magic happens. But it operates a little differently in concept.

### WHY WAS IT INVENTED?

IPsec is actually not one protocol but a **protocl suite**, a bunch of protocols that work together to service a machine coherently. IPsec was developed in the mid-1990 in order to provide security at the IP layer, and secure communications through authentication and encryption. Basically it serves the same purpose as TLS/SSL but on the 3rd layer instead of the 5th.

### HOW DOES IT WORK?

IPsec suite provides **Integrity**, **Authentication**, **Confidentiality** to any Layer 3 communication that uses it. IPsec also provides it's own encryption's **Key Management Services**, how it does all of that is by using different protocols:

* **`Authentication Header [AH]`**                Provides authentication and integrity.
* **`Encapsulation Security Protocol [ESP]`**     Provides authentication, integrity and confidentiality (encryption).
* **`Internet Key Exchange [IKE]`**               Used to negotiate the *Security Association* (SA).

Both AH and ESP are encapsulation protocols, an IPsec VPN may use one or both of them. IKE has two versions, V1 and V2 which are a little different so let's get into it.

> #### `IKEv1.0`
> ###### *[#UDP/500]*
> 
> ##### First Phase
> 
> The IKE protocol performs what is similar to a handshake, both peers in the communicaion need to agree on five `Security Association`s in order to create a `VPN tunnel`:
> 
> * **H ashing algorithm** for message integrity checking.
> * **A uthentication** for Proving the other side is who he says he is.
> * **G roup** agreeing on the Diffie-Hellman group to use.
> * **L ifetime** how long should the tunnel stay up for?
> * **E ncryption algorithm** for consealing message data.
> 
> H.A.G.L.E, it's an easy accronym to remember (not).
>
> Also, the negotiation, or rather IKEv1 Phase 1 can happen in two modes:
> * **`Main Mode`** The identity of both peers is encrypted and annonymous.
> * **`Agressive`** The identity of both peers is cleartext and public (not secure).
> 
> Let me explain things a little bit more:
>
> The **Authentication** SA is the way that both machines can authenticate the identity of the other machine, that can be using a digital certificate or RSA.
>
> The **Group** SA is the group number to use when exchanging keys using Diffie-Hellman, I wont go into the Diffie-Hellman protocol. Just know that a higher number means a amore secure key.
>
> So after agreeing on the five `SA`s, which we will refer to as a `Policy Set`, both peers create a securly encrypted communication "tunnel". First they authenticate eachother, and then they `create a shared secret key` using Diffie-Hellman and the `Policy Set`. Ontop of that *first tunnel* they will initiate the second phase of the IKE protocol.
>
> ##### Second Phase
> 
> The goal of the second phase is to negotiate the `scope` of the tunnel, "What data to protect? and How?", the goal of phase 1 is to protect phase 2.
> Negotiations on this phase construct a `new SA set` called the `Transform Set`. The transform set includes:
>
> * **Encapsulation Protocol** [AH] or [ESP].
> * **Encryption algorithm** the last one was to create the phase 1 tunnel, this one is to create the final tunnel.
> * **Hashing algorithm** for message integrity checking.
> * **Tunnel Mode** eiter `Transport` or `Tunnel`.
>
> Tunnel modes describe the way the packet will be transformed, `Tunnel mode` means the **entire packet will be encrypted** and a **new IP header** will be added to provide routing, that means that NAT is possible using this mode. `Transport mode` the packet keeps it's original IP header for routing, this mode doesn't protect the entire packet but only the transport layer payload.
> 
> Using this `Transform Set` IPsec "constructs a new tunnel" over which it will send the data securely and the conversation will begin.


> #### `IKEv2.0`
> ###### *[#UDP/500] [#UDP/4500]*
>
> IKE version 2 is faster than version 1, provides `NAT Traversal`, provides a `keep-alive` mechanism, supports the `**Extensible Authentication Protocol [EAP]**`, reconnecting to a tunnel after changing networks, stronger cryptoraphic algorithms (PFS).
> The goal of both IKE protocols is exactly the same, since IKEv2 is a little more complicated and does exactly the same I will not go into the exact process. Just know that IKEv1 sends 6 or 9 messages and IKEv2 sends only 4 messages.
> 
> Lastly let's talk about the other protocols

> The biggest difference between the two protocol is that [ESP] provides encryption and [AH] does not.
>
> #### Authenticationi Header [AH] Protocol
>
> This protocol's goal is to provide message integrity and authentication, it does that by adding a header to the packet called the Authentication Header. In the header will be a `Hash or MD` (Message Digest) of the TCP/IP header (depending on the tunneling mode) + A shared key. When the peer receives the packet it will compare the digest/hash in the authentication header with the message hash/digest that it will generate itself. This proves integrity if the messages are the same and proves authenticity if the calculation with the shared key succeeded on both sides.
> 
> #### Encapsulation Security Protocol [ESP]
>
> This protocol also does what the AH protocol does but it also encrypts the message (ESP supports AES, DES & 3DES). ESP adds an `ESP Trailer` to the IP/TCP packet (depending on tunneling mode) and encrypts that, afterwards it will add an `ESP Header`.
> ```
>  _________2.DIGESTED__________
> |        _____1.ENCRYPTED_____|
> '_______'______ _______ ______'
> |_ESP_H_|_IP_H_|_TCP_H_|_Data_|
> This diagram shows ESP encapsulation in 'tunnel' tunneling mode.
> ```
> Then the protocol `hashes/digests the entire packet` with the `shared key` and puts the hash/digest in a new `ESP Authentication` Trailer. Lastly in the tunneling mode is *Tunnel* the protocol adds a new IP Header.
>
> The peer will decrypt the message, and then check the digest against a digest it will generate on it's own to affirm integrity and authentication. But more over, a succeful decryption of the message in itself proves the authenticity of the sender.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Secure Shell A.K.A SSH
###### *[#Layer-7] [#TCP/22]*

SSH was conceptualized as a secure replacement for the [telnet](#teletype-network-aka-telnet) protocol. The SSH protocol allows a user to remotely interface with another machine on the internet with their communication secured.

### WHY WAS IT INVENTED?

The protocol was invented by Tatu Ylönen after he found a password sniffer on his university's network. Since the prominent remote managment protocol back then was Telnet, all communications appeared as cleartext on the wire. That pushed Tatu to design the first version of the protocol all the way back in 1995.

### HOW DOES IT WORK?

SSH is actually a suite of SSH protocols riding on TCP, each playing a key part in the role of remote managment. Those protocols are:

```
  ______________________________________
 /                 /                   /| SSH TRANSPORT LAYER PROTOCOL - Provides server authentication, message integrity, compression and data
/_________________/___________________/ |                                confidentiality. (encryption)
|  SSH USER AUTH  |  SSH CONNECTION   | | SSH USER AUTHENTICATION PROTOCOL - Provides user authentication services.
|    PROTOCOL     |     PROTOCOL      | / SSH CONECTION PROTOCOL - Allows multiplexing a single connection to multiple logical channels. 
|_________________|___________________|/.
 /                                     /|
/_____________________________________/ |
|                                     | |
|    SSH TRANSPORT LAYER PROTOCOL     | /
|_____________________________________|/.
 /                                     /|
/_____________________________________/ |
|                                     | |
|    TRANSMISSION CONTROL PROTOCOL    | /
|_____________________________________|/
```

Alright, first step! 

### Connection Initialization & Key exchange
```
Since I want to draw another diagram I will do this in a code box. Let's try         C                                 S
and do this simply step by step. The client will want to initiate a connection.      | Identification String Exchange  |
The client will send an 'Identification String Exchange' message, to                 | ------------------------------> |
synchronize the SSH version. The server will return the same message.                | <------------------------------ |
                                                                                     |                                 |
Afterwards comes the `Key Exchange Initialization` message, just like in TLS         |   Key Exchange Initialization   |
the client and server will send their ciphers in preffered order and more            | ------------------------------> |
parameters that the exchange requires like hashing & compression algorithms.         | <------------------------------ |
Both client and server will do this at about the same time.                          |                                 |
                                                                                     |                                 |
After agreeing on a cipher, hashing & compression algorithm the client will send     |                                 |
an `ECDH Key Exchange Initialization` message to start a diffie hellman Key          |                                 |
Exchange procedure. In this step both client and server will generate the same key   |                                 |
using emphereal public and private they create and send eachother. They will also    |     ECDH Key Exchange Init      |
sign all of their messages up until now together with their emphereal public key     | ------------------------------> |
and the secret key as a hash, the server will sign their hash with their private     |     ECDH Key Exchange Reply     |
rsa key.                                                                             | <------------------------------ |
                                                                                     |                                 |
First the server has to accept the server's public rsa key, either by a certificate  |                                 |
or a `public key database`. If bot of those don't succeed the client can decide      |                                 |
wether to accept the conection or not.                                               |                                 |
                                                                                     |                                 |
The client can decrypt then hash with the server's public rsa key and                |                                 |
match the server's hash to a hash they generate themselves the exact same way.       |                                 |
That way the client proves both the server has the same key and that the server is   |                                 |
the server.                                                                          |                                 |
                                                                                     |                                 |
The client and sever will not actually communicate with the shared secret key, they  |                                 |
will use that key to derive 6 new keys. 2 keys for `encryption`, 2 keys as `Init     |             New Keys            |
Vectors`, 2 keys for `Integrity` (like, SSL MAC). the client will send an encrypted  | ------------------------------> |
`New Keys`.                                                                          |     SSH_MSG_SERVICE_REQUEST     |
                                                                                     | ------------------------------> |
And to end it, the client send an `SSH_MSG_SERVICE_REQUEST` message to ask for the   |     SSH_MSG_SERVICE_ACCEPT      |
authentication service. The server will return an `SSH_MSG_SERVICE_ACCEPT` response  | <------------------------------ |
and the user will be allowed to log in.                                              V                                 V
```

### SSH Transport Layer Protocol

The transport layer protocol is responsible for a few things, some of them we alredy touched already.

> #### `Integrity, Compression, and Confidentiality of data`
>
> Here is a diagram to explain how the protocol does that:
> ```
> seq# - packet sequence no.      ____________________SSH_PACKET____________________
> pktl - packet len              |__________________2.MAC_____________________      |
> pdl  - padding len             |       _____________1.ENCRYPTED_____________|     |
>  _________________             '______'______ _____ ______________ _________'_____'
> |_____PAYLOAD_____|            |_seq#_|_pktl_|_pdl_|_CMPRSSD_PYLD_|_padding_|_MAC_|
>          '--------------------------------------------'
> ```

> #### `Saving Host Keys`
>
> This protocol needs to save the host keys that he different hosts (clients) connecetd to the ssh'd machine (server).
> It can do that by relying on a CA and matching client Certificates, or just keeping them in an internal Database.

> #### `Key Generation`
>
> The protocol is also responsible for the key generation, that is generate the 6 keys from the exchanged secret key.
> The key exchange intself is also done by the `SSH Transport Layer Protocol`.

### SSH User Authentication Protocol

This protocol role is to authenticate users wanting to connect to the server. The client sends an authentication request message with the username credential. The server will then offer the client authentication methods:
* Password - client inputs a password, the password in encrypted in transit.
* Public Key - client authenticates themselves using their public key.
* Host Based - client authenticates themselves using a central host (Like a DC).

The client may authenticate themselves by any of these methods, the protocol will return a success response and authenticate the user. Or if the authentication is not enough, the server may ask the client to further authenticate themselves.

### SSH Connection Protocol

This protocol rides ontop of the `SSH Transport Layer Protocol`. The protocol is responsible for many of the things that make SSH so great.

> #### `Multiplexing`
>
> This protocol is responsible for opening and closing channels for concurrent data transfer, there are 4 types of channels:
> * Session - used in remote execution of a program (like the shell), file transfer, etc.
> * X11 - Used to send desktop interface to remote machines, for applications that show graphical interface of the server machine.
> * forwarded-tcpip - remote port forwarding, we will get to that in a second.
> * direct-tcpip - local port forwarding.

> #### `Port forwarding`
>
> SSH enables port forwarding on an encrypted ssh tunnel to protect communication, SSH allows both remote and local port forwarding.
> * local - local port forwarding let's an applicaiton forward a port onto ssh directly onto a port of the server.
> * remote - remote port forwarding let's an application forward information on the ssh tunnel to an intermediate server (like a firewall) that will send the information to the ssh client on the server.
>
> Remote port forwarding is used when direct communication to the server is impossible, for example when there is a firewall. The `SSH client` on the client machine will `encrypt the data` and the `ssh server` on the server machine will `decrypt the data` before forwaring it to the server application.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Remote Desktop Protocol A.K.A RDP
###### *[#layer-7] [#tcp/3389]*

RDP is a protocol that allows an interface to a remote windows machine with input devices (keyboards & mice) and output devices (monitors, speakers and headsets).

### WHY WAS IT INVENTED?

The remote desktop feature was first pushed out with the release of Windows NT 4.0, called `Remote Desktop Protocol 4.0`. The protocol was not created for a particular purpose but it is heavily used nowadays for remote machine intefaces. 

### HOW DOES IT WORK?

RDP uses a bundle of protocols to achieve the final product that is RDP.

```
  __________________________________________
 /                                         /|
/_________________________________________/ |
|       RDSTLS/SSL/CredSSP (Encrypt)      | |  | An encryption protocol. 
|_________________________________________|/|
| Multipoint Communication Service (MCS)  | |  | A protocol that lets the connection become multiplexed, meaning communication
|                  T.125                  | |  | through multiple channels.
|_________________________________________|/|
| Connection-Oriented Transport Protocol  | |  | A connection-oriented Transport Protocol. Used for the transport of packets.
|                  X.224                  | |
|_________________________________________|/|
|                   TPKT                  | |  | A transport protocol for the OSI model, performs a different encapsulation.
|_________________________________________|/   | Uses TCP for transport but works with a different architechture.
```

The data in the RDP session is funneled through the 7 layer OSI model then it is sectioned, directed to a channel, encrypted, wrapped, framed and packaged before sending.

#### RDP Connection

To initiate a connection RDP performs a sort of handshake.

```
.     Cli                                           Ser
.      |                                             |
.      | ------------------------------>             | ] Connection -
.      |             <------------------------------ | ] Initiation
.      |                                             |
.      | ------------------------------>             | ] Basic Settings -
.      |             <------------------------------ | ] Exchange
.      |                                             |
.      | ------------------------------>             | ] Channel -
.      |             <------------------------------ | ] Connection
.      |                                             |
.      | ------------------------------>             | ] Security commencement
.      |                                             |
.      | ------------------------------>             | ] Secure Settings Exchange
.      |                                             |
.      |             <------------------------------ | ] Licensing
.      |                                             |
.      |             <------------------------------ | ] Capabilities -
.      | ------------------------------>             | ] Exchange
.      |                                             |
.      | ------------------------------>             | ] Connection -
.      |             <------------------------------ | ] Finalization
.      |                                             |
.      | ------------------------------>             | ] Data -
.      |             <------------------------------ | ] Exchange
.      |                                             |
.      V                                             V
```

> #### `Connection Initialization`
>
> The connection is initiated by sending a request using the X.224 protocol.
> The request contains some flags and the security protocols supported by the client. 
>
> The connection is confirmed once the server sends an x.224 packet back with a negotiation response.
> That response will contain the selected security protocol.
>
> *From now on each message will be wrapped using the x.224 protocol.*

> #### `Basic Settings Exchange`
>
> The client and server start communicating some basic settings using MSC by sending initialize and response PDUs (Protocol Data Units).
> These basic settings contain: graphics settings, some security data like encrytion methods and randoms, information about what channels the client wants to open.
> The server will open those channels and returns the client channels IDs.

> #### `Channel Connection`
>
> The client and server will now communicate back and forth and will connect every single channel that the client requested in the last stage.
> The client will send a request to "connect" a specific channel, the server will do so and to confirm will return that channel's id.
>
> *From now on each message will be wrapped in an MSC PDU and directed to a virtual channel*

> #### `Security Commencement`
>
> The client will send it's own random encrypted with the server's public key, now both peers can calculate a symmetric key.
> *From now on, subsequent traffic can be encrypted.*

> #### `Secure Settings Exchange`
>
> The client sends information about the user (domain, username, password etc.) and supported compression methods.

> #### `Licensing`
>
> In this stage the RDP server (RDS) will verify it's liense to connect more than 2 machine simultaneously. In case it does not have the protper license it will support only up to 2 connections.

> #### `Capabilities Exchange`
>
> The server will send a PDU containing a structure that has many capability sets like: general, input, fonts, virtual channels and more (overal 28 types).
> Then the server will send a PDU with the display monitors' information to the client.
> Finally, the client will repond with a PDU containing it's owht structure of capability sets.

> #### `Connection Finalization`
>
> This step is comprised of many little steps. First the client and server synchronize user identifiers. Then they both send a PDU to indicate shared control over the session. Then client asks for control over the session and the server grants it.

After finishing the handshake the client and server will communicate graphical data (from the server) and input data (from the client) on their respective channels. So a packet will look something like this:

```
 _________________________________________________________________________________________
| Encrypted  ________________________________________________________________________     |
|           | OSI over TCP (X.224) ________________________________________________  |    |
|           |                     | MCS Layer (data is put into channel)           | |    |
|           |                     |                __________________              | |    |
|           |                     |               | PDU (data)       |             | |    |
|           |                     |               |__________________|             | |    |
|           |                     '________________________________________________' |    |
|           '________________________________________________________________________'    |
'_________________________________________________________________________________________'
```

Obviously this is not the exact structure of an RDP packet, since the entire packet can not be encrypted for sake for transmission. But it does ilustrate the structure of "How does the protocol encapsulate it's information".


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Microsoft SQL Server Tubular Data Stream A.K.A MS-SSTDS
###### *[#Layer-7] [#TCP/1433]*

The ms-sstds (also known as mssql, and will be refered to as fro now on) protocol is a protocol that allows authentication, query and other operations on a microsoft sql server.

### WHY WAS IT INVENTED?

The protocol was created in order to let a client to interface with an ms sql server.

### HOW DOES IT WORK?

The protocol operated on a client server model, in which the client will send a request to the server and the server will perform the operation the client requested and return a response. This protocol also performs authentication.

The ms-sql session is directly tied to the transport layer protocol's session, and it does not matter what transport layer protocol is used. The protocol also provides an optin for encryption with TLS/SSL.

Let's go into details:

> #### `Authentication`
>
> Before performing operations or sending sql statements to be executed the client must authenticate itself to the server.
> Before "logging in", the client will send a `PRELOGIN` message. This message will contain some context for the session, and also may be used to wrap the TLS handshake payload. Then the server will return a `PRELOGIN response`.
>
> Afterwards the client must send a `LOGIN` message in order to authenticate, this message will contain the username and password of the user. The server will return a `LOGIN response`, that messagewill contain information about the server if the login is successful.

> #### `Client` `SQL Batch`
>
> When the client wants to send the server sql statements (an sql batch) it will bundle them up in a 'batch' and send them.

> #### `Server` `Row Data`
>
> The server will answer `SQL Batch` messages that return data with the data that was requested stored in rows.

> #### `Client` `Remote Procedure Call (RPC)`
>
> If the client wishes to perform an rpc call on the sever then it can send an isolated rpc call packet.

> #### `Server` `Remote Procedure Call (RPC) Response`
>
> The server can return data or status of the executed RPC call back to the client.

> #### `Attention`
>
> This message can be used to terminate the last operation that was requested.

Overall, the flow of a connection may look something like this:

```
.     Cli                                           Ser
.      |                                             |
.      |                                             |
.      | ------------------------------>             | ] TCP
.      |             <------------------------------ | ] Handshake
.      |                                             |
.      |                   PRELOGIN                  |
.      |                                             |
.      | ------------------------------>             | ] PRELOGIN
.      |             <------------------------------ | ] 
.      |                                             |
.      | ------------------------------>             | ] PRELOGIN wrapping TLS
.      |             <------------------------------ | ] 
.      |                                             |
.   ---+------------------ENCRYPTED------------------+---
.      |                                             |
.      |                    LOGIN                    |
.      |                                             |
.      | ------------------------------>             | ] LOGIN
.      |             <------------------------------ | ]
.      |                                             |
.      |                  POST-LOGIN                 |
.      |                                             |
.      | ------------------------------>             | ] SQL Batch
.      |             <------------------------------ | ] Row data
.      |                                             |
.      | ------------------------------>             | ] RPC call
.      |             <------------------------------ | ] RPC data response
.      |                                             |
.      |                     ...                     |
.      |                                             |
.   ---+---------------UNTIL-DISCONNECT--------------+---
.      |                                             |
.      |                                             |
.      |                                             |
.      V                                             V
```


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Modbus

Modbus is a data communication protocol. Modbus provides a common language for devices and equipment to communicate with one another, those devices are usually `PLC` (Programmable Logic Controller) or `SCADA` (Supervisory Control And Data Acquisition) devices. To save any confusion, Modbus is a standalone protocol that does not belong to the TCP/IP or OSI model. However, there is a Modbus version for Ethernet.

### WHY WAS IT INVENTED?

Modbus was invented by Modicon in 1979 to be used by it's PLCs. Modbus allows devices with different interfaces to communicate over a **common language**. Today it is a widely used protocol in the industry since it is open source and free to use, that way any engineer can simply pick it up and use it in their own equipment.

### HOW DOES IT WORK?

The Modbus protocol operates in a `client/server` model, the client is the one **recieving** the information and the server is the one **sending** the information (a bit confusing I know). The information, in the form of bits, is sent over the serial lines. The modbus protocol can operate in two different modes, RTU will send information in the form of binary over the wire, ASCII will send information in the form of ASCII over the wire.

The way that message work in RTU mode is as follows:

```
 ___________________________________________________
| 1  byte | 1  byte |       n bytes       | 2 bytes |
|_srv_adr_|_opcode__|_Data________________|_CRC_____|
```

Let's break it down.

> #### `Server Address`
>
> A modbus network is made up of up to 248 machines, 1 client and up to 247 servers. Each server in the network has it's own address, so the first byte in the message will be the server's address. When a server receives a message it will read the first byte and will be able to understand whether to drop the packet or not.

> #### `Opcode`
>
> The opcode is a byte sized value that represents a specific operation like read from a specific table/coil. Data is stored inside of the server in the form of registers/coils, coils store analog data (1 or 0, true or false), registers store numerical values. There are four tables to keep those values and each value has it's address:
>
> | Coil/Register Numbers | Addresses   | Type        | Table Name                          |
> | --------------------- | ----------- | ----------- | ----------------------------------- |
> | 1 - 9999              | 0000 - 270E | Read/Write  | Output Coils                        |
> | 10001 - 19999         | 0000 - 270E | Read        | Input Coils                         |
> | 40001 - 49999         | 0000 - 270E | Read/Write  | Output Registers                    |
> | 30001 - 39999         | 0000 - 270E | Read        | Input Registers                     |
>
> | Action          | Table             | Code  |
> | :-------------- | :---------------- | :---- |
> | Read            | Output Coil       | 01    |
> | Read            | Input Coil        | 02    |
> | Read            | Output Register   | 03    |
> | Read            | Input Register    | 04    |
> | Write Single    | Output Coil       | 05    |
> | Write Single    | Output Register   | 06    |
> | Write Multiple  | Output Coil       | 0F    |
> | Write Multiple  | Output Register   | 10    |
>
> So a message that start with `3203..` tells the server to *read* from the *Output Register* in the server with the *id* of 50 (32 in HEX), *the value address will also be given in the message*.

> #### `Modbus Map`
>
> A modbus map is a table that maps the kind of data inside the tables. Addresses to data types, size and byte orders.
> Some machiens may have a pre-built given by the manufacturer, while others let the operator program their own modbus maps.

> #### `CRC`
>
> Stands for Cyclic Redundancy Check, it is a digest of the entire message inside of the two last bytes of the message. The client will then digest the recieved message and compare the result to the last two bytes to see if there were mishaps in transport.

Using these formats in the messages the client and servers can communicate reliably and efficiently.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## BIBLIOGRAPHY
This bibliography was put together after writing the NTP section, so most of the earlier protocol's research resources are missing.

> ### ICMP
>
> 1. "[INTERNET CONTROL MESSAGE PROTOCOL, DARPA INTERNET PROGRAM PROTOCOL SPECIFICATION](www.rfc-editor.org/rfc/rfc792)"
> , Request For Comments 792.

> ### HTTP
>
> 1. "[Hypertext Transfer Protocol Version 2 (HTTP/2)](www.rfc-editor.org/rfc/rfc7540)", Refrence For Comments 7540.
> 2. "[HTTP response status codes](developer.mozilla.org/en-US/docs/Web/HTTP/Status#successful_responses)", Article on **MDN Web Docs**.
> 3. "[HTTP request methods](developer.mozilla.org/en-US/docs/Web/HTTP/Methods)", Article on **MDN Web Docs**.

> ### SMTP
>
> 1. "[What is SMTP - Simple Mail Transfer Protocol](www.youtube.com/watch?v=PJo5yOtu7o8)", Youtube video by **PowerCert Animated Videos**.
> 2. "[SMTP ( Simple Mail transfer protocol )](www.youtube.com/watch?v=I2JtOjU5MZI)", Youtube video by **Easy Engineering By Neha Syed**.

> ### POP
>
> 1. "[POST OFFICE PROTOCOL](datatracker.ietf.org/doc/html/rfc918)", Request For Comments 918.
> 2. "[E-Mail Protocols (SMTP, POP and IMAP)](www.youtube.com/watch?v=m8TLN--aZic&t=1s)", Youtube video by **Rick Graziani**.

> ### IMAP
>
> 1. "[INTERNET MESSAGE ACCESS PROTOCOL - VERSION 4](www.rfc-editor.org/rfc/rfc1730.txt)", Request For Comments 1730.

> ### SMB
>
> 1. "[Visuality Systems SMB protocol](www.youtube.com/playlist?list=PLyOlunpO5LG1W1SgFGDUAlCTSz9j9zBax)"
> , Youtube Playlist by **Visuality Systems**.

> ### SNMP
>
> 1. "[SNMP Explained | Simple Network Management Protocol | Cisco CCNA 200-301](www.youtube.com/watch?v=Lq7j-QipNrI)"
> , Youtube Video by **CertBros**.

> ### NTP
>
> 1. "[NTP Explained | Network Time Protocol | Cisco CCNA 200-301](www.youtube.com/watch?v=oCtkwEjhyD4)", Youtube video by **CertBros**.

> ### TLS/SSL
> 
> 1. "[TLS Handshake - EVERYTHING that happens when you visit an HTTPS website](www.youtube.com/watch?v=ZkL10eoG1PY)"
> , Youtube video by **Practical Networking**.
> 2. "[Transport Layer Security, TLS 1.2 and 1.3 (Explained by Example)](www.youtube.com/watch?v=AlE5X1NlHgg)"
> , Youtube video by **Hussein Nasser**.
> 3. "[What is SSL? | What is SSL Ceritificate? | SSL Architecture and Protocols | Secure Socket Layer](www.youtube.com/watch?v=-dQyUqoK16w)
> ", Youtube video by **Chirag Bhalodia**.
> 4. "[How SSL works tutorial - with HTTPS example](www.youtube.com/watch?v=iQsKdtjwtYI)", Youtube video by **tubewar**.
> 5. "[The Secure Sockets Layer (SSL) Protocol Version 3.0](www.rfc-editor.org/rfc/rfc6101)", Request For Comments 6101.
> 6. "[Difference between Secure Socket Layer (SSL) and Transport Layer Security (TLS)](www.geeksforgeeks.org/difference-between-secure-socket-layer-ssl-and-transport-layer-security-tls/)", Article on **Geeks-For-Geeks**.
> 7. "[THE EVOLUTION OF SSL AND TLS](www.digicert.com/blog/evolution-of-ssl)", Article on **DigiCert**.
> 8. "[The TLS Protocol Version 1.0](www.ietf.org/rfc/rfc2246.txt)", Request For Comments 2246.
> 9. "[TLS / SSL Versions - Part 2 - Practical TLS](www.youtube.com/watch?v=fk0-UqwVNqY)", Youtube Video by **Practical Networking**.
> 10. "[TLS 1.3 Handshake](www.youtube.com/watch?v=yPdJVvSyMqk)", Youtube video by **F5 DevCentral**.

> ### IPsec
>
> 1. "[CCNP Security | IKEv1 Phase 1 and Phase 2 Explained](www.youtube.com/watch?v=GGB8cvN6AQI)", Youtube video by **CCNADailyTIPS**.
> 2. "[IPsec - IKE Phase 1 | IKE Phase 2](www.youtube.com/watch?v=tapoOQ-MkPU)", Youtube video by **GD Networking Newbie**.
> 3. "[Authentication Header (AH) and Encapsulating Security Payload (ESP)](www.youtube.com/watch?v=ScxCFzxVel8)"
> , Youtube video by **GD Networking Newbie**.
> 4. "[QTNA #37: IPSec modes](www.youtube.com/watch?v=HbaUqhYZjq4)", Youtube video by **CyberVista**.
> 5. "[Module 5 Lecture 1 IP Security: Operation Modes-Transport Mode and Tunnel Mode](www.youtube.com/watch?v=uVmkL8uPZPk)"
> , Youtube video by **Eupheus Mnemon**.
> 6. "[001 IPSEC KEv2](www.youtube.com/watch?v=LHnnzdipaKQ)", Youtube video by **Sikandar Shaik**.
> 7. "[002 IKEv1 vs IKEv2](www.youtube.com/watch?v=UEimHi_imsk)", Youtube video by **Sikandar Shaik**.

> ### SSH
>
> 1. "[How SSH Works](www.youtube.com/watch?v=5JvLV2-ngCI)", Youtube video by **Mental Outlaw**.
> 2. "[9 - Cryptography Basics - SSH Protocol Explained](www.youtube.com/watch?v=0Sffl7YO0aY)", Youtube video by **CBTVid**.
> 3. "[SH | SSH Protocol Stack | SSH Protocols explain with animation | Why SSH? | Secure Shell (SSH)](www.youtube.com/watch?v=jA1IoEHcrIU)", Youtube video by **Chirag Bhalodia**.

> ### RDP
>
> 1. "[Explain Like I’m 5: Remote Desktop Protocol (RDP)](www.cyberark.com/resources/threat-research-blog/explain-like-i-m-5-remote-desktop-protocol-rdp)", Article on **CyberArk**.

> ### LDAP
>
> 1. "[What is LDAP and Active Directory ? How LDAP works and what is the structure of LDAP/AD?](www.youtube.com/watch?v=QyhNaY5O468)", Youtube video by **Talented Developer**.
> 2. "[Lightweight Directory Access Protocol (LDAP): The Protocol](www.rfc-editor.org/rfc/rfc4511)", Request For Comments 4511.

> ### MS-SQL
>
> 1. "[[MS-SSTDS]: Tabular Data Stream Protocol Version 4.2](learn.microsoft.com/en-us/openspecs/sql_server_protocols/ms-sstds/dab36a48-6c13-44c7-954a-0f5c8623590d)", Article on **MSDN**.

> ### Modbus
>
> 1. "[What is Modbus and How does it work?](www.se.com/us/en/faqs/FA168406/)", Article on **Schneider Electric**.


---------------------------------------------------------------------------------------------------------------------------------------------------
###### [Back to top](#bigous-protocolous)