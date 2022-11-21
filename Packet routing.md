# HOW PACKETS GET THRUOGH THE NETWORK

In my summary of the `7 Layer OSI Model` I talked about 3 tehcniques that packets use to get routed through the internet, `Hop by Hop`, `End to End` and `Service to Service`. These techniques facilitate the ability to get a message from one machine to another, and the way they work is enabled by the layers they lie in.

When passing in the network the packets will be processed and transformed by different network devices, so to know what happens to a packet we needs to learn and understand those devices:

## Switch

The switch is the devices needed to physically connect multiple computers to a LAN (a Local Area Network, A hub can also be used to do that but it is very stupid device and generally not used). The devices connect to the switch used the network cables, the slot that they use to physically connect to the switch is called `a port`. Actually the switch isn't really aware of the existance of other devices, it just routes packets through itself based on the MAC address written as the destination address in the L2 header of that packet.

```
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4                    ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```

The way the routing works is through a process of translating MAC addresses to switch ports, **a single port can be bound to multiple MAC addresses**. And the translation happens inside of a think called the `MAC Address Table`, essentially the MAC address table is like a telephone book of MAC addresses that tells it what port to use to reach that address. The MAC Address Table begins empty.


| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| Empty         |               |


A switch performs three actions. First, **Learning**:
> When the switch recieves a packet with **an unknown MAC address** it will register it in the MAC address table. The switch will take the `source MAC address` of the packet and the `port it was recieved on` and will write those both down as bound in the MAC address table.



But what happens when a packet needs to be sent to a MAC address that isn't recognized by the switch, then it's only option is to perform it's second operation and that is **Flooding**. The switch will send the packet to of all the ports except the port that it was sent from, when the endpoints at the other sides of the ports recieve the packet they will inspect the L2 header and only accept the packet if it matches their own MAC adress, otherwise they will drop it silently.

If a packet arrives to the switch with a MAC address that is registered it will translate that MAC address using the MAC address table and **Forward** that packet to the designated port.

===

Let's take the network shown above and demonstrate, `HOST A` wants to send a packet to `HOST B`. `HOST A` 
```
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====     --PACKET-->    P3 | <--- | P4    ?               ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2               ?  /                \  ?               b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```
The switch learns HOST A's MAC Address to it's `MAC ADDRESS TABLE`.

| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| a2a2          | P1            |

Then it flood the packet to the network since it doesn't know the MAC Address translation of "b3b3".
```
SWITCH -.
        |
        :---------> P2
        :---------> P3
        '---------> P4

.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4    --PACKET-->V    ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====   X<--PACKET--                        --PACKET-->X   ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```
*Notice the packet is only accepted on HOST B*

And this is how the packet reaches HOST B, however also notice that `the Switch did not learn the port that HOST B belongs to`.
Now what if `HOST B` wants to reply to `HOST A`, it will send a packet back with `HOST A`'s MAC Address in the L2 Header.

```
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====                    P3 | <--- | P4    <--PACKET--     ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```
The switch will then **learn** `HOST B`'s MAC Address and map it in the MAC address Table. and also translate the destination MAC address to port 1.

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

Where it will then **Forward** the packet.

```
.         |    __                        .------.                        __
.         |   [Ll] ___________________P1 | ---> | P2___________________ [Ll]
.         |   ====     <--PACKET--    P3 | <--- | P4                    ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```