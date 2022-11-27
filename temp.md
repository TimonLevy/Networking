# BIGOUS PROTOCOLOUS
---------------------

## ICMP

ICMP, or **I**nternet **C**ontrol **M**essage **P**rotocol is a 3rd (OSI Model) layer protocol used to send control messages relating to the Internet protocol (IP).

### Why Was It Invented?
Since the implementation of IP is not meant to be absolutely reliable and it can sometimes mess up, The ICMP protocol be used to return send feedback about problems that can arise when communicatinf with a host. However the ICMP protocl does not act to make the IP more reliable and does not guarantee 100% reliablility itself incase IP fails to deliver.

### How Does It Work?
An ICMP packet is used to instruct a device in the routing path of a packet (including the source and destination) of a failure or change in the delivery of the packet (Except for `ICMP Type`s 0 and 8). The type and structure of the packet is determined by the `ICMP Type` parameter in the header. Each type also has an `ICMP Code` that determines the subject of the packet.

#### ICMP Type Specifications
[0 & 8] Echo & Echo Reply

An ICMP Echo is used to check the connectivity from one endpoint to another, and ICMP Echo Reply is used to reply to Echo requests. Endpoints can send an `ICMP Echo Request` to another endpoint which will get routed to them and if the destination machine recieves the request it will send back an `ICMP Echo Reply`.

Fields:
> **Address**
> 
> The destination address in an Echo request will be the destination of the Echo Reply message. 

> **Type**
> 
> 8 Echo Request
> 
> 0 Echo Reply

> **Code**
>
> 0 (Default value)

> **Checksum**
>
> The checksum is the 16-bit ones's complement of the one's complement sum of the ICMP message starting with the ICMP Type.

> **Identifier**
>
> An identifier to aid in matching echos and replies (for example port number for control messages relating to a service), may be zero.

> **Sequence Number**
>
> A sequence number to aid in matching echos and replies (May be an incremental value for control messages sent in the same stream), may be zero.