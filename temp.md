### Hop by Hop

I already gave a little explanation as to how that works in the OSI summary but here is the details. What we refer to as a hop is that transmission of the packet from one NIC to the next NIC in the chain that eventually leads to the target machine that uses the destination IP address. Communication between NICs is that the layer 2 header and MAC Addresses exist for, basically how the technique works is:

Let's say PC 1 wants to send a message to PC 2, **PC 1 must know the IP address of PC 2**.
PC 1 includes the `MAC address of NIC 2` in it's **L2 Header** and `PC2's ip address` into it's **L3 Header**.

```
PC  1        Device 2     PC 2       
NIC 1 -----> NIC 2 -----> NIC 3
.      HOP          HOP
.
Packet:
MAC - Router
IP  - PC2
```

Afterwards, when `Device 2` will get the packet it will check the L2 header and remove it since it fulfilled it's purpose. And it will attach it's own L2 header to forwad the packet forward to the next link in the chain. Every machine that exists in 3 and above has a storage table called an `ARP TABLE` where it stores the `MAC Address to IP Address translations` of all the devices that it is connected to and interacted with.
(Let's say for example that PC 2's ip is reistered on NIC 2's device's ARP Table)
So the MAC Address it will use for it's new L2 header will be taken from the ARP table, the one that matches PC 2's L3 IP Address.

```
PC  1        Device 2     PC 2       
NIC 1 -----> NIC 2 -----> NIC 3
.      HOP1         HOP2
.
.            Packet:
.            MAC - PC2
.            IP  - PC2
```

Then `Device 2` will send the packet with the new L2 header and that will reach PC 2 where it will check the L2 Header, see that is correct and strip it. It will then check the L3 header, see that is correct and strip that too and recieve the information.

## SCENARIO 1, FROM THE POV OF HOST A

```
MACHINE   |   HOST A                                                 HOST B
.         |    __                                                      __
.         |   [Ll] __________________________________________________ [Ll]
.         |   ====                                                    ====
IP ADDR   |   10.1.1.22                                               10.1.1.37
MAC ADD   |   a2a2                                                    b3b3
```

`HOST A` wants to send a ping to `HOST B`.
* HOST A has to know HOST B's IP address, either by:
  * Use input. (manually entering the address in the ping command)
  * Address-name resolution. (DNS, LLMNR, NBNS)
* HOST A **does not** know HOST B's MAC address.

1. HOST A sends an `ARP request for HOST B's MAC address` in a broadcast, in the request it will include it's own IP/MAC addresses:

```
MACHINE   |    HOST A                                                 HOST B
.         |    __                                                      __
.         |   [Ll] __________________________________________________ [Ll]
.         |   ====                       ---->                        ====
IP ADDR   |   10.1.1.22              ARP BROADCAST                    10.1.1.37
MAC ADD   |   a2a2                                                    b3b3

+-------------------------------+
| Address Resolution Protocol   |
| (Request)                     |
|-------------------------------|
| Hello, if 10.1.1.37 is out    |
| there please tell me your MAC |
| address. I am 10.1.1.22/a2a2. |
|                               |
'-------------------------------'
| LAYER 2 HEADER                |
|-------------------------------|
| SRC MAC                  a2a2 |
| DST MAC         ffffff:ffffff | // MAC Address "ffffff:ffffff" stands for broadcast, this means
'-------------------------------' // that the message will reach all machines in the lan.

```

2. `HOST B` gets the arp request that `HOST A` sent, adds HOST A's IP/MAC addresses to their ARP cache and replies with an ARP reply directly to HOST A in a unicast:

```

MACHINE   |   HOST A                                                 HOST B
.         |    __                                                      __
.         |   [Ll] __________________________________________________ [Ll]
.         |   ====                       <----                        ====
IP ADDR   |   10.1.1.22               ARP UNICAST                     10.1.1.37
MAC ADD   |   a2a2                                                    b3b3

+-------------------------------+
| Address Resolution Protocol   |
| (Reply)                       |
|-------------------------------|
| I am 10.1.1.37 my MAC is b3b3 |
|                               |
'-------------------------------'
| LAYER 2 HEADER                |
|-------------------------------|
| SRC MAC                  b3b3 |
| DST MAC                  a2a2 |
'-------------------------------'

```

Then `HOST A` write `HOST B`'s IP/MAC addresses to it's own ARP cache and sends the ICMP request:

```
MACHINE   |   HOST A                                                 HOST B
.         |    __                                                      __
.         |   [Ll] __________________________________________________ [Ll]
.         |   ====                       <----                        ====
IP ADDR   |   10.1.1.22               ICMP UNICAST                    10.1.1.37
MAC ADD   |   a2a2                                                    b3b3

+-------------------------------+
| Internet Control Message      |
| Protocol (request)            |
|-------------------------------|
| "Hello are you alive?"        |
|                               |
'-------------------------------'
| LAYER 3 HEADER                |
|-------------------------------|
| SRC IP              10.1.1.22 |
| DST IP              10.1.1.37 |
'-------------------------------'
| LAYER 2 HEADER                |
|-------------------------------|
| SRC MAC                  a2a2 |
| DST MAC                  b3b3 |
'-------------------------------'

```

## HOW A SWITCH LEARNS MAC ADDRESSES

A switch learns MAC addresses by simply looking at the layer 2 header of packets when they pass thorugh.

```
MACHINE   |  HOST A                       switch                       HOST B
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4                    ====
MAC ADD   |   a2a2                   /   '------'   \                   b3b3
.         |  HOST C                 /                \                 HOST D
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MAC ADD   |   d7d7                                                      f1f1
```
| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| Empty         |               |

This is our network layout, a LAN with four machines.

1. `HOST A` sends a packet to `HOST B`, in this case, HOST A knows HOST B's **MAC ADDRESS**.

```
MACHINE   |  HOST A                       switch                       HOST B
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====     --PACKET-->    P3 | <--- | P4    ?               ====
MAC ADD   |   a2a2                   /   '------'   \                   b3b3
.         |  HOST C              ?  /                \  ?              HOST D
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MAC ADD   |   d7d7                                                      f1f1
.         |
.         |   a2a2(HOST A) --> SWITCH -->?? b3b3(HOST B)
```

The switch does not know what port the MAC address belongs toso it floods the packet.

```
SWITCH -.
        |
        :---------> P2
        :---------> P3
        '---------> P4
```

The switch also adds HOST A's MAC Address to it's ARP TABLE.

| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| a2a2          | P1            |

Each host checks the layer 2 header to verify that the packet was meant for them and if not, they will drop it.
This means that `the only recipient of he message will be HOST B`.

Now, when HOST B sends packet back (maybe a reply) to HOST A.

```
                THE ROUTER SENDS THE PACKET TO ALL THE CONNECTED MACHINES

MACHINE   |  HOST A                       switch                       HOST B
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4    --PACKET-->V    ====
MAC ADD   |   a2a2                   /   '------'   \                   b3b3
.         |  HOST C                 /                \                 HOST D
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====   X<--PACKET--                        --PACKET-->X   ====
MAC ADD   |   d7d7                                                      f1f1



MACHINE   |  HOST A                       switch                       HOST B
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4    <--PACKET--     ====
MAC ADD   |   a2a2                   /   '------'   \                   b3b3
.         |  HOST C                 /                \                 HOST D
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MAC ADD   |   d7d7                                                      f1f1
```

It will then find the MAC address on it's `MAC ADDRESS TABLE` and send it to the mapped port, it also adds the new MAC address to that table.

```
b3b3(HOST B) --> a2a2(HOST A)
.                 ^
.         .-------'
.         |
```

| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| a2a2          | P1            |
| b3b3          | P2            |

```
MACHINE   |  HOST A                       switch                       HOST B
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====      <--PACKET--   P3 | <--- | P4                    ====
.         |  HOST C                  /   '------'   \                  HOST D
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MAC ADD   |   d7d7                                                      f1f1

```

