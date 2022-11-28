# BIGOUS PROTOCOLOUS
---------------------

## Internet Control Message Protocol A.K.A ICMP

ICMP, or **I**nternet **C**ontrol **M**essage **P**rotocol is a 3rd (OSI Model) layer protocol used to send control messages relating to the Internet protocol (IP).

### WHY WAS IT INVENTED?
Since the implementation of IP is not meant to be absolutely reliable and it can sometimes mess up, The ICMP protocol be used to return send feedback about problems that can arise when communicatinf with a host. However the ICMP protocl does not act to make the IP more reliable and does not guarantee 100% reliablility itself incase IP fails to deliver. ICMP will usually report on delivery errors of packets and no ICMP packet will be sent about a lost ICMP packet.

### HOW DOES IT WORK?
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

ICMP Header
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
> `Address`
> 
> The destination address in an Echo request will be the destination of the Echo Reply message. 

ICMP Fields:
> `Type`
> |   |                                     |
> |---|-------------------------------------|
> | 8 | Echo Request                        |
> | 1 | Echo Reply                          |

> `Code`
>
> 0 (Default value)

> `Identifier`
>
> An identifier to aid in matching echos and replies (for example port number for control messages relating to a service), may be zero.

> `Sequence Number`
>
> A sequence number to aid in matching echos and replies (May be an incremental value for control messages sent in the same stream), may be zero.

#### [3] Destination Unreachable

An ICMP `Destination Unreachable` message will be recieved when a packet can't be delievered because a waypoint couldn't route it, the destination host service could not accept the packet (IP protocol or port isn't active), the gateway determined that the destination is unreachable or a DF (Dont Fragment) interuppted a fragmented packet and it was dropped due to it.

IP Fields:
> `Destination Address`
>
> The address of the original packet sender.

ICMP Field:
> `Type`
>
> 3

> `Code`
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

#### [11] Time Exceeded

An ICMP `Time Exceeded` if a packet's TTL reaches 0 while traveling to it's destination, the final waypoint it encoutered may send a Time Exceeded message to the sender host, or, if the desination host cannot reassemble a fragmented packet in the time limit it may send a Time Exceeded to the sender host.

IP Fields:
> `Destination Address`
>
> The address of the original packet sender.

ICMP Field:
> `Type`
>
> 11

> `Code`
>
> |   |                                     |
> |---|-------------------------------------|
> | 0 | ttl exceeded in transit             |
> | 1 | fragment reassembly time exceeded   |
>
> Code 0 can be sent from a waypoint, code 1 can be sent from a host.


---------------------


## Domain Name System A.K.A DNS
The DNS protocol is used for "translating" between IP addresses and Domain Names, thus the name Domain Name System. The protocol itself operates at the 7th OSI level which is the application layer and ontop of `UDP/53`. DNS went under a lot of transformation over the year since it was first introduced at 1983, today it exists in many variants that each serve a different purpose or build on previous implementation of the protocol. Some of these are:

> `DNS over TCP/53`
>
> Provides more stability, longer answers and re-use of long-lived connections between clients and servers.

> `DNS over TLS` (TCP/853)
>
> Provides encryption to the entire DNS session, requires a dedicated DoT server.

> `DNS over HTTPS` (TCP/443)
>
> Tunnels DNS over HTTPS.

> `DNS over QUIC` (UDP/853)
>
> Uses the transport protocol QUIC to transport messages, provides a lot of benefits that TCP does with encryption and speed.

And there are more. 


### Why Was It Invented?
Computers can not communicate with domain names alone, computers needs Ip addresses to be able to do that. But humans can't remember IP addresses very well, unlike Domain names. Imagine that if you wanted to visit the youtube homepage on your browser you'd have to type youtube's raw ip, It would be annoying and hard to remember, additionaly you'd have to do the same for every other site you want to visit which is practically impossible and absolutely uncomfortable for the end user.

One of the most common use case of DNS is resolving URLs to ip addresses, But not only. DNS doesn't only deal with URLs, it can actually translate the name of any computer that is registered into a `DNS server` but we will get to that later. And DNS can also resolve ip addresses into Domain Names the other way around.

### How does it work?

#### DNS Tables and Cache
The first step of the translation actually happens on the host, the host will try to resolve the domain name by looking in their DNS cache. If they can't find the site name in there then they will send a DNS request to their **assigned DNS Server**. DNS works on a request response basis, meaning I can **send a server a request (DNS Query)** and **it will give me a response (DNS Response)**. When the server recieves the request with the ip it will look into it's own `DNS Table` to try to resolve the address.

A `DNS Table` is a long list of **DNS records**, basically a list of the Domain name to IP solution **it knows**. A DNS record can be of several types but we will focus on the main ones, '`A`' and '`PRT`'. 'A' is the type that defines Domain Name to Address resolution, this record will be used to translate a given domain name to it's corresponding ip ('A' for IPv4, 'AAAA' for IPv6). A 'PTR' type record will do the opposite and resolve the address into a domain name. DNS can also resolve email addresses to route outgoing emails to an SMTP email server using the `MX` type and for other services using the `SRV` type.

#### DNS Query Types
There are multiple types of querys that the host can ask of the server to perform in order to resolve the address.

> `Recursive Query`
>
> In a recursive query the DNS server tries to find the **authoratative** DNS server of the address (The DNS server containing the resolution), through a recursive process starting at the Root DNS server. The server *must* return a response, either the resolution or an error.

> `Non-Recursive Query`
>
> In this query either the DNS server has to have the dns resolution or it queries the authoratative server for that hostname (which it must be familiar with through an NS type record).

> `Iterative Query`
>
> The iterative query will try to return the best answer from it's cache and if it cant it will redirect the client to the DNS Root Server or the Authoratative server for that resolution where the client will have to repeat the query.

#### Getting a response
Finally, the server will return a reponse to the client which it will use. Either the server gives the client the resolution IP or it will error, in case the server does resolve the address the Client will `keep that resolution in it's own DNS Cache` to make future resolution of that name instantaneous.


---------------------


## Dynamic Host Configuration Protocol A.K.A DHCP

DHCP is used to assigned unique IP addresses to hosts in the network automatically, it can also assign other network configuration like subnet mask, DNS server and Default gateway. DHCP can provide a client with all the parameters it needs in order to operate in the network.

### Why Was It Invented?
Originally, when setting up a new netowrk or adding a new computer to an existing network the IP address, Subnet Mask, Default Gateway and DNS Server Address needed to be put manually into the new machine's network configuration (ncpa.cpl). This kind of IP assignment is known as `Static IP`. However that is uncomfortable, messy and hard to manage. DHCP solves this issue by assigning machine `Dynamic IP Leases` and configuring all the essentials automatically.

### How Does It Work?
DHCP works on Server-Client framework, dhcp server services can be run on host machines but also on routers.
The DHCP addresses that are given to the machines are merely `leased`, meaning they will expire after a given time and the client will have to renew their lease. That is done in order to retrieve IP addresses of disconnected machines since the server cannot know when or if they were disconnected and no longer occupy the address.

DHCP Servers offer up IP addresses based on a configured `DHCP Scope` on the DHCP Server. A DHCP Scope is a range dictated by a **Start and End Address and subnet mask**. It will look into the subnet that the client is located at and at the available IP addresses and offer one of those. DHCP Scopes have to be manually configured.

Machines on the subnet may also reserve addresses on the DHCP Server in order to always recieve the same address, the reservation is made using the MAC address.

### IP Assignment Process
> `DHCPDISCOVER`
>
> When a client detects that it has no IP (And DHCP is enabled on it) it will look for a DHCP server on the network using a broadcast message. The DHCPDISCOVER message MAY include options that suggest values for the network address and lease duration.

> `DHCPOFFER`
>
> Each DHCP Server that the message gets to may reespond with an available IP address to the client in DHCPOffer message.

> `DHCPREQUEST` 
>
> The client can send a DHCPREQUEST message to a server in order to:
> * Accept/Decline a DHCPOFFER, a client will accept one offer and deny all other.
> * Request a extension to an existing lease.

> `DHCPACK`
> 
> Server sends network configuration parameters, including committed network address.

at the end, when a computer wants to disconnect from the network it can send.
> `DHCPRELEASE`
> 
> Client to server relinquishing network address and cancelling remaining lease.