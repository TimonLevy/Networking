# ARP

ARP is a layer 2 Protocl that bridges between the layer 2 (Ethernet) and layer 3 (IP) addressing schemes. Since there is no direct forula used for "translation" between MAC and IP addresses ARP is used to fill that gap.

To route packets through a network, both an IP address and a MAC address are needed, an IP address of the final destination of the packet and the MAC address of the next hop in the route to that final destination dictated by the IP address. When one of these factors are unknown ARp is used to solve for it.

The way it works is by sending a request to the entire network in a broadcast asking who the holder of the IP/MAC address is, so that they can provide the missing address. To further understand let's imagine a scenario:

> An endpoint doesn't know the MAC address of their default gateway to route the message to the internet. That endpoint will send an ARP request to the LAN asking for the MAC of the default gateway.

The protocol's packet format is as follows

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Hardware Type               | Protocol Type                 |
+---------------+-------------+-------------------------------+
| Hw Addr Len   | Pr Addr Len | Opcode                        |
+-----------------------------+-------------------------------+
| Sender Hardware Address (size is written in L2 Addr Len)    |
|                             +-------------------------------|
|_____________________________|                               |
| Sender Hardware Address (size is written in L3 Addr Len)    |
+-------------------------------------------------------------+
| Target Protocol Address (size is written in L2 Addr Len)    |
|                             +-------------------------------|
|_____________________________|                               |
| Target Protocol Address (size is written in L3 Addr Len)    |
+-------------------------------------------------------------+
```

The `Hardware Type` field is for different types of Layer 2 address formats, together with the `Hardware Address Length` it is used to decide the address in the Sender & Target Hardware address. Same goes for the `Protocol Type` and `Protocol Address Length`.

An Ethernet (Hardware) & IP (Protocol) request can be summed up this way: "Who has [IP/MAC Address]? I am [My IP Address] [My MAC Address]." (In broadcast)
And a reply will be: "I am [IP/MAC Address], my MAC/IP Address is [Address]."

# Vulnerability

There is a problem with this system however, ARP responses may be accepted by machines without them ever sending a request. And that may lead to a malicious change in the victim's ARP table. One that let's the attacker become a middlemen! An attacker can then diguise themselves as another machine, and actch all the network directed to that machine. Once they have that network they can alter it, block it or simply just read it.

How I did it.