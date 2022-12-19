# BIGOUS PROTOCOLOUS

The big protocol wikipedia.

| TABLE OF CONTENTS | _________________________________________________________________________________________________________________ |
| :---------------- | ----------------------------------------------------------------------------------------------------------------: |
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
| [NTP](#network-time-protocl-aka-ntp)                                  | Network Time Protocol                                         |
| [TLS/SSL](#transport-layer-security--secure-socket-layer-aka-tlsssl)  | Transport Layer Security / Secure Socket Layer                |

---------------------------------------------------------------------------------------------------------------------------------------------------

## Internet Control Message Protocol A.K.A [ICUP](https://www.urbandictionary.com/define.php?term=Spell%20iCup)
###### *[#Layer-3] [#Portless]*

ICMP, or **I**nternet **C**ontrol **M**essage **P**rotocol is a 3rd (OSI Model) layer protocol used to send control messages relating to the Internet protocol (IP).

### WHY WAS IT INVENTED
Since the implementation of IP is not meant to be absolutely reliable and it can sometimes mess up, The ICMP protocol be used to return send feedback about problems that can arise when communicatinf with a host. However the ICMP protocl does not act to make the IP more reliable and does not guarantee 100% reliablility itself incase IP fails to deliver. ICMP will usually report on delivery errors of packets and no ICMP packet will be sent about a lost ICMP packet.

### HOW DOES IT WORK
An ICMP packet is used to instruct a device in the routing path of a packet (including the source and destination) of a failure or change in the delivery of the packet (Except for `ICMP Type`s 0 and 8). The type and structure of the packet is determined by the `ICMP Type` parameter in the header. Each type also has an `ICMP Code` that determines the subject of the packet.

### PACKET FORMAT

```
IP Header
|- Version                                      4
|- IHL                                          Internet Header Length in 32-bit words
|- Type of Service                              0
|- Total Length                                 Length of internet header and data in octets
|- Identification, Flags, Fragment Offset       
|- Time To Live                                 The amount ofhops remaining for the packet until a waypoint/host drops it
|- Protocol                                     ICMP = 1
|- Header Checksum                              The 16 bit one's complement of the one's complement sum of all 16 bit words
|                                               in the header
|- Source Address
'- Destination Address

I C U P Header
|- Type                                         8  Bits, determines the format of the rest of the data.
|- Code                                         8  bits, determines message type code to provide additional infomartion.
'- Checksum                                     16 Bits, The checksum is the 16-bit ones's complement of
                                                the one's complement sum of the ICMP message starting
                                                with the ICMP Type

ICMP Data
|- Pointer                                      32 bits, Pointer to the byte location in the IP Header of the faulty packet.
'- Original IP message content                  Original IP Header + Datagram data (Up to 576 bytes)
```

### ICMP TYPE SPECIFICATIONS

#### `[0 & 8] Echo & Echo Reply`

An ICMP Echo is used to check the connectivity from one endpoint to another, and ICMP Echo Reply is used to reply to Echo requests. Endpoints can send an `ICMP Echo Request` to another endpoint which will get routed to them and if the destination machine recieves the request it will send back an `ICMP Echo Reply`.

IP Fields:
> #### `Address`
> 
> The destination address in an Echo request will be the destination of the Echo Reply message. 

ICMP Fields:
> #### `Type`
> |   |                                     |
> |---|-------------------------------------|
> | 8 | Echo Request                        |
> | 1 | Echo Reply                          |

> #### `Code`
>
> 0 (Default value)

> #### `Identifier`
>
> An identifier to aid in matching echos and replies (for example port number for control messages relating to a service), may be zero.

> #### `Sequence Number`
>
> A sequence number to aid in matching echos and replies (May be an incremental value for control messages sent in the same stream), may be zero.

#### `[3] Destination Unreachable`

An ICMP `Destination Unreachable` message will be recieved when a packet can't be delievered because a waypoint couldn't route it, the destination host service could not accept the packet (IP protocol or port isn't active), the gateway determined that the destination is unreachable or a DF (Dont Fragment) interuppted a fragmented packet and it was dropped due to it.

IP Fields:
> #### `Destination Address`
>
> The address of the original packet sender.

ICMP Field:
> #### `Type`
>
> 3

> #### `Code`
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

IP Fields:
> #### `Destination Address`
> 
> The address of the original packet sender.

ICMP Fields:
> #### `Type`
> 
> 11

> #### `Code`
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
> ######*[#UDP/137]*
> 
> NetBIOS provides it's own type of network architechture where each computer is represented by a 16 byte `name`, 15 ASCII characters to represent the computer and the last as a bit-field that represents what type of services the machine provides. The name will then be registered to an ip address (Only IPv4), will will enable name resolution.
> 
> NBNS (NetBios Name Service) also acts as an "Alternative" method of name resolution incase DNS and LLMNR both fail, The names are resolved by WINS (Windows Internet Naming Service) Servers. Where, instead of Domain names the server resolves NB names to IP addresses. The Servers are used to balance networking loads and to avoid flooding the network with broadcasts.
> 
> NOTICE: NBT Names are apply only to the link-local network, they are not to be confused with domain names.

> #### `NetBIOS Session Service`
> #####*[\#TCP/139]*
> 
> Applications using NetBIOS can establish a NetBIOS over TCP (NBT) session with a "call" command, then communicate with "send" and "recieve" commands. Since NBSS uses TCP as it's base it allows for bigger messages, transmission control and loss recovery.

> #### `NetBIOS Datagram Service`
> ######*[\#UDP/138]*
> 
> Applications may also communicate over NetBIOS connectionless, using individual datagrams over the NetBIOS Datagram Service. The datgram services allows unicast, multicast and broadcast datagram messaging, using "group ips", an application can listen on a designated group ip to recieve a satagram sent to that group's members.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Teletype Network A.K.A Telnet
###### *[#Layer-5] [#TCP/23]*

Telnet is a session layer protocol that enables terminal access to an application on another host. Allowing the user to send commands to another endpoint rmotely.

### WHY WAS IT INVENTED?

The idea for a terminal that can interface with the network was proposed on [rfc 15](https://www.rfc-editor.org/rfc/rfc15.html "Network Subsystem for Time Sharing Hosts") back in 1969. The documents discusses the idea of remotely accessing another machines terminal abstractly and even having the ability to transfer files over the network. The host machine would just send each chacrater input to the remote machine, and the host machien would have a special format to ented basic commands.

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
The protocol to transfer files over the net was first prposed in 1971 on [rfc 114](https://www.rfc-editor.org/rfc/rfc114), the document discusses the usability of a protocol to act as an abstraction layer when a user from one machine wants to retrieve a file from another machine to his own while not being familiar with the other machine's file system and interactive commands (unlike Telnet). Basically, the protocol is supposed to act as a middle man for transferring files quickly and efficiently between one host and another that knows both host's languages and provides it's own commands. The protocol also allows remote storage of files.


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
###### *[#Layer7] [#TCP/80]*

HTTP is the protocol used to send web pages over the internet. This is a very broad definition of the protocol but we will get into it later (Like I did to your mom).

### OVERVIEW

Http evolved so much over the years so I'll ty to summarize it to as little as possible.

Overall, HTTP works as a request response type protocol, just like dns and dhcp. The whole point of it is to get the hosted webpage from the server, like "www.google.com" where I research this stuff or "jokes.moomoo.co.il" where I get my your mom jokes.

[In the beninging](https://www.youtube.com/watch?v=fWKB8zdVM-U) the internet was in it's diapers, all you could find are super simple webpages like blogs. There was no need for a complicated protocol to transfer this data and that's where HTTP came in.
![The Original Spacejam Website](https://www.themarysue.com/wp-content/uploads/2010/12/SpaceJam_Home.png)
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
> * **Persistent connections** - HTTP now keeps it's connection alive after sending a request and doesn't open a new session for each request (Which has an RTT of 3 thanks to the TCP handshake).
> * **6 New methods** that were really helpful in the usage of the protocol. (PUT, PATCH, DELETE, CONNECT, TRACE, OPTIONS)
>
> Though the protocol **still communicated over cleartext**, which wasn't very cool.

> #### `HTTP/2.0`
>
> This protocol ought to make things a whole lot better:
> * **Multiplexing** - Now you can send multiple requests on multiple concurrent connections that represent a single session.
> * **Prioritizing** - Sending more that one request? Tell what you want first!
> * **Automatic Compression** - Automatic [Gzip](https://10web.io/site-speed-glossary/gzip-compression/) compression.
> * **Connection reset** - The optino to terminate and reestablish a connection with the server, if for some reason you need it.
> * **Server Push** - Let the server give you all the commonly requested data before you even ask for it, this let's it load balance.


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
###### *[#Layer7] [TCP/110]*

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
###### *[#Layer7] [#TCP/143] [#TCP/993] [[#What_Came_In_The_Mail?](https://youtu.be/CjgXx6QyZRg?t=10)]*

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
###### *[#Layer7] [#TCP/445] [#TCP/139]*

SMB is a protocol meant for file sharing, not to be confused with file transfer. This protocol allows a user on a Windows machine to share a folder over the network and let other user iteract with it on their local machine without downloading it. It works on a client/server model and uses commands to make the other endpoint operate.

### WHY WAS IT INVENTED?
The protocol was invented on 1983 by Barry A. Feigenbaum at IBM in order to provide Hosts, printers and other network nodes `shared access` to files on the network. The protocol was then picked up by Microsoft and even since then it was buit upon with the 4th version of the protocol being the current standard. The protocl was also imitated and ported over to Linux to provide Unix based systems access to file sharing and interfacing abilities with Windows machines.

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
###### *[#Layer7] [#UDP/161]*

The Simple Network Management Protocol is used to collect device information, like cpu usage, up status and bandwith from all the devices over the network to allow simple network managment from one machine very easily.

### WHY WAS IT INVENTED?
The protocol was invented back in 1988, back then networks weren't as big as they are today but they were getting bigger. This is why there was a need for a way to manage IP networks easily, a need which was fullfilled by this protocol.

### HOW DOES IT WORK?

> #### `MODEL`
> 
> This protocl uses a Controller-Agent model, and it operates on the UDP transport protocol. As to how it get's the information is pretty simple, every device on the network has a table called a `MIB` (Management Information Base). In the MIB will be a list of `Object identifiers`, `Objects` are like properties of the network device, an Object can be the name of the device or a routing table of a router.

> #### `Server & Requests`
>
> The controlling server, called `NMS` (Network Management System) can then fetch the information from the MIBs using `GET requests` (Yes, like http you nerd). The NMS may also change the values of the objects on the agent's machine using `SET requests`.

> #### `TRAP/INFORM messages`
>
> If an interface goes offline on a specific device containing an snmp agent, the agent will send either a `TRAP` or `INFORM` alert to the NMS to notify it. Both alerts server the same purpose but are still a little different, that is in the way that they are communicated. An `INFORM alert` will wait for acknowledgment from the server and will resend if it didn't get a message back saying it was acknowledged, a `TRAP alert` will send a forget.

So basically using these operations and with a MIB table on all devices a network administrator can display all the information about their network's devices, and get notified when a device doesn't work as needed.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Network Time Protocl A.K.A NTP
###### *[#Layer7] [#UDP/123]*

The protocol is used to synchronize time over all the machines in the network, time synchronization is crucial when it comes to many things in a network like logging events. in order to do that an NTP server will dictate the time for all the machines in the network. 

### HOW DOES IT WORK?

The way time synchronization works over the internet is by devices syncing their internal clocks with eachother, the reliability of the device's clock sncronization is called `stratum`. `Stratum` is measured from 0 - 15, devices that classify as `stratum 0` are usually professional grade time measurement devices like `Atom clocks` and such, those will connect to `Time servers` which will become `stratum 1` since they sync off of a `stratum 0` device.

NTP Servers on small networks will usually syncronize using stratum 1,2 or 3 servers off the internet and then the other devices on te network will syncronize off of that NTP server.


###### [Back to top](#bigous-protocolous)
---------------------------------------------------------------------------------------------------------------------------------------------------


## Transport Layer Security **/** Secure Socket Layer A.K.A TLS/SSL
###### *[#Layer5]*

Both TLS and SSL are protocols used to encrypt information on the wire in a way that both endpoints can decrypt them, meaning that `they generate a set of symetric encryption eys for the given session`. That is why they belong in the `Session Layer`, the one that is also responsible for encryption.

Nowadays TLS is the standard for session encryption and regarded as the better protocol, originally TLS was born from SSL and superseeded it.

### WHY WERE THEY INVENTED?

At the time, most communication through the network happened as cleartext, there was a serious threat from middlemen existing in the network reading all the information. Especially when it came to businesses and online transactions. So in the year 1995 [Netscape](https://www.youtube.com/watch?v=dQw4w9WgXcQ) introduced this protocol to serve as a porm of packet protection by encrypting packet contents on the wire.

### HOW DO THEY WORK?

#### SSL












## BIBLIOGRAPHY
This bibliography wasput together after writing the NTP section, so most of the earlier protocol's research resources are missing.

> ### ICMP
>
> 1. "[INTERNET CONTROL MESSAGE PROTOCOL, DARPA INTERNET PROGRAM PROTOCOL SPECIFICATION](https://www.rfc-editor.org/rfc/rfc792)", Request For Comments.

> ### SMTP
>
> 1. "[What is SMTP - Simple Mail Transfer Protocol](https://www.youtube.com/watch?v=PJo5yOtu7o8)", Youtube video by **PowerCert Animated Videos**.
> 2. "[SMTP ( Simple Mail transfer protocol )](https://www.youtube.com/watch?v=I2JtOjU5MZI)", Youtube video by **Easy Engineering By Neha Syed**.

> ### POP
>
> 1. "[POST OFFICE PROTOCOL](https://datatracker.ietf.org/doc/html/rfc918)", Request For Comments.
> 2. "[E-Mail Protocols (SMTP, POP and IMAP)](https://www.youtube.com/watch?v=m8TLN--aZic&t=1s)", Youtube video by **Rick Graziani**.

> ### IMAP
>
> 1. "[INTERNET MESSAGE ACCESS PROTOCOL - VERSION 4](https://www.rfc-editor.org/rfc/rfc1730.txt)", Request For Comments.

> ### SMB
>
> 1. "[Visuality Systems SMB protocol](https://www.youtube.com/playlist?list=PLyOlunpO5LG1W1SgFGDUAlCTSz9j9zBax)", Youtube Playlist by **Visuality Systems**.

> ### SNMP
>
> 1. "[SNMP Explained | Simple Network Management Protocol | Cisco CCNA 200-301](https://www.youtube.com/watch?v=Lq7j-QipNrI)", Youtube Video by **CertBros**.

> ### NTP
>
> 1. "[NTP Explained | Network Time Protocol | Cisco CCNA 200-301](https://www.youtube.com/watch?v=oCtkwEjhyD4)", Youtube video by **CertBros**.

> ### TLS/SSL
> 
> 1. "[TLS Handshake - EVERYTHING that happens when you visit an HTTPS website](https://www.youtube.com/watch?v=ZkL10eoG1PY)", Youtube video by **Practical Networking**.
> 2. "[What are SSL/TLS Certificates? Why do we Need them? and How do they Work?](https://www.youtube.com/watch?v=r1nJT63BFQ0)", Youtube Video by **Hussein Nasser**.
