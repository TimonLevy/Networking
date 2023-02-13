# Promiscuous Mode

Promiscuous mode is a setting on a Network Interface Card that tells it not to drop packets that were not destined to it. A NIC drops packet when the destination ip of the pecket is not the same as the ip on the NIC itself, however wth promiscuous mode, a NIC will not do that but accept incoming traffic.

Promiscuous mode isn't some alien technology that let's you sniff all packets going through the network, they still have to get to your NIC in order to sniff them. That means that we can perform some countermeasures:

1. Switch, the switch forwards packets to the destination machines. Meaning that the promiscuous sniffer will only get packets destined to him anyways, and not anyone else's.
2. If the Promicuous sniffer decides to perform some attack to bypass the switch (arp spoofing, ip spoofing, icmp redirection) we can try to fight those with the switch configurations as well. We may decide to limit a switch port to only 1 MAC address if we know it isn't going to be used by another switch, that way the attacker wont be able to perform arp spoofing very well.