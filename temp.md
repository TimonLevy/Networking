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
