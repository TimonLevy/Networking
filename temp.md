# BIGOUS PROTOCOLOUS
---------------------

## ICMP

ICMP, or **I**nternet **C**ontrol **M**essage **P**rotocol is a 3rd (OSI Model) layer protocol used to send control messages relating to the Internet protocol (IP).

### Why Was It Invented?
Since the implementation of IP is not meant to be absolutely reliable and it can sometimes mess up, The ICMP protocol be used to return send feedback about problems that can arise when communicatinf with a host. However the ICMP protocl does not act to make the IP more reliable and does not guarantee 100% reliablility itself incase IP fails to deliver. ICMP will usually report on delivery errors of packets and no ICMP packet will be sent about a lost ICMP packet.

### How Does It Work?
An ICMP packet is used to instruct a device in the routing path of a packet (including the source and destination) of a failure or change in the delivery of the packet (Except for `ICMP Type`s 0 and 8). The type and structure of the packet is determined by the `ICMP Type` parameter in the header. Each type also has an `ICMP Code` that determines the subject of the packet.

**ICMP Packet Format**

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


#### ICMP Type Specifications
**[0 & 8] Echo & Echo Reply**

An ICMP Echo is used to check the connectivity from one endpoint to another, and ICMP Echo Reply is used to reply to Echo requests. Endpoints can send an `ICMP Echo Request` to another endpoint which will get routed to them and if the destination machine recieves the request it will send back an `ICMP Echo Reply`.

IP Fields:
> **Address**
> 
> The destination address in an Echo request will be the destination of the Echo Reply message. 

ICMP Fields:
> **Type**
> |   |                                     |
> |---|-------------------------------------|
> | 8 | Echo Request                        |
> | 1 | Echo Reply                          |

> **Code**
>
> 0 (Default value)

> **Identifier**
>
> An identifier to aid in matching echos and replies (for example port number for control messages relating to a service), may be zero.

> **Sequence Number**
>
> A sequence number to aid in matching echos and replies (May be an incremental value for control messages sent in the same stream), may be zero.

**[3] Destination Unreachable**

An ICMP `Destination Unreachable` message will be recieved when a packet can't be delievered because a waypoint couldn't route it, the destination host service could not accept the packet (IP protocol or port isn't active), the gateway determined that the destination is unreachable or a DF (Dont Fragment) interuppted a fragmented packet and it was dropped due to it.

IP Fields:
> **Destination Address**
>
> The address of the original packet sender.

ICMP Field:
> **Type**
>
> 3

> **Code**
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

**[11] Time Exceeded**

An ICMP `Time Exceeded` if a packet's TTL reaches 0 while traveling to it's destination, the final waypoint it encoutered may send a Time Exceeded message to the sender host, or, if the desination host cannot reassemble a fragmented packet in the time limit it may send a Time Exceeded to the sender host.

IP Fields:
> **Destination Address**
>
> The address of the original packet sender.

ICMP Field:
> **Type**
>
> 11

> **Code**
>
> |   |                                     |
> |---|-------------------------------------|
> | 0 | ttl exceeded in transit             |
> | 1 | fragment reassembly time exceeded   |
>
> Code 0 can be sent from a waypoint, code 1 can be sent from a host.