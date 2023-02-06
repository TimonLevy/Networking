# ICMP

ICMP is a layer 3 protocol that is responsible for notifying machine on the network about problems with the IP protocol, as well and managing it.

First, ICMP stands for `Internet Control Message Protocol`. The way it operates is simple, whenever there is an incident with the IP protocol, a message is sent to notify the sending computer of it. An ICMP packet is built very dynamically, there are 3 main fields in it and the rest are dependant.

## Types

Each ICMP message has a code that defines what the meaning of the message is. Here's a list:

| Type | Message                 |
| ---: | :---------------------- |
|    0 | Echo Reply              |
|    3 | Destination Unreachable |
|    4 | Source Quench           |
|    5 | Redirect                |
|    8 | Echo                    |
|   11 | Time Exceeded           |
|   12 | Parameter Problem       |
|   13 | Timestamp               |
|   14 | Timestamp Reply         |
|   15 | Information Request     |
|   16 | Information Reply       |

There were more messages added later on but we will not elaborate on them in this summary.

The most used ICMP messages are the `Echo & Echo Reply` message and the `Destination Unreachable` message.

> #### `Echo & Echo Reply`
>
> The Echo message is an "alive-check", when an echo message gets sent to a machine it will send back an echo reply message. It is a way to know if there is connectivity to a machine. That is what the ping program does.  
>
> Note: If a machine doesn't reply to an echo it does not neccesarily mean that there is no connectivity, it might be a firewall issue.

> #### `Destination Unreachable`
>
> A destination unreachable message is sent when the target machine of a packet above layer 3 is well... unreachable, this message will be sent by a router.

And a bonus message.

> #### `Redirect`
>
> A Redirect message is sent when a router (Likely the LAN's default gateway) routes a packet through another router on the same LAN as the sender machine, in that case it would be better off to send the packet straight to that other router and save a hop. So, the router will send a redirect message to the sender machine and alter their routing table so that next time they will send it to the other ruter immediately.

## Codes

Codes are just like types, but for types. Each message type has different codes that are used to define even more what is the problem. Like, message type `Redirect` has 4 codes (0, 1, 2, 3) that tell it to redirect for a whoe network, a specific host, a specific service from a networ or a specific service from a host. 

`Destination Unreachable` also has codes (0, 1, 2, 3, 4, 5) for if a net (0), host (1), protocol (2) or a port (3) are unreachable, fragmentation mishap (4) or a routing failure (5).

If a type has no codes then that field will just be 0.

## The rest of the fields

The rest of the field take up the rest of the packet's buffer. The buffer is 64 bits long, 8 for Type, 8 for Code and 16 for checksum leave 32 bits for the other fields which are defualtly reserved. That is because the developers of the protocol thought that someday more types will added so dynamics are required.

The reserved 32 bit buffer is allocated according the type and code, if `Redirect` is sent then the 32 bits will be used to save the IP address of the preffered gateway. If `Destination Unreachable` is sent, the 32 bits are left empty since they have no use.

# ICMP Redirection Attack

An ICMP redirection attack takes advantage of the ICMP redirect message, and an attacker tries to place himself as a gateway for a victim machine on the local network. It is very simple.

