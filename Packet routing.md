# HOW PACKETS GET THROUGH THE NETWORK

In my summary of the `7 Layer OSI Model` I talked about 3 techniques that packets use to get routed through the internet, `Hop by Hop`, `End to End`, and `Service to Service`. These techniques facilitate the ability to get a message from one machine to another, and the way they work is enabled by the layers they lie in.

When passing in the network the packets will be processed and transformed by different network devices, so to know what happens to a packet we need to learn and understand those devices:

## Switch

The switch is the device needed to physically connect multiple computers to a LAN (a Local Area Network, A hub can also be used to do that but it is a very stupid device and generally not used). The devices connect to the switch using network cables and the slot that they use to physically connect to the switch is called `a port`. The switch isn't aware of the existence of other devices, it just routes packets through itself based on the MAC address written as the destination address in the L2 header of that packet.

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

The way the routing works is through a process of translating MAC addresses to switch ports, **a single port can be bound to multiple MAC addresses**. And the translation happens inside of a thing called the `MAC Address Table`, essentially the MAC address table is like a telephone book of MAC addresses that tells it what port to use to reach that address. The MAC Address Table begins empty.


| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| Empty         |               |


A switch performs three actions. First, **Learning**:
> When the switch receives a packet with **an unknown MAC address** it will register it in the MAC address table. The switch will take the `source MAC address` of the packet and the `port it was received on` and will write both down as bound in the MAC address table.

But what happens when a packet needs to be sent to a MAC address that isn't recognized by the switch, then its only option is to perform it's second operation and that is **Flooding**. The switch will send the packet to all of the ports except the port that it was sent from, when the endpoints at the other sides of the ports receive the packet they will inspect the L2 header and only accept the packet if it matches their own MAC address, otherwise they will drop it silently.

If a packet arrives at the switch with a MAC address that is registered it will translate that MAC address using the MAC address table and **Forward** that packet to the designated port. Lastly, it doesn't matter to the switch what device is on the other end of a port. It can be a computer, a router, or even another switch, all the switch looks at is the MAC address, the L2 Header.

Most communication happens through the switch but keep in mind that it can also occur **with** the switch, and that is possible because **a switch has its own MAC and IP addresses**, so when communicating with a switch it essentially acts like any other computer in the networking aspect.

-------------------------------------------------------------------------------------------------------------------

Let's take the network shown above and demonstrate, `HOST A` wants to send a packet to `HOST B`. `HOST A` 
```
.         |    __                        .------.                        __
.         |   [Ll] __________________ P1 | ---> | P2 __________________ [Ll]
.         |   ====     --PACKET-->    P3 | <--- | P4    ?               ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2               ?  /                \  ?               b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```
The switch learns HOST A's MAC Address to its `MAC ADDRESS TABLE`.

| MAC ADDRESS   | PORT          |
| ------------- | ------------- |
| a2a2          | P1            |

Then it floods the packet to the network since it doesn't know the MAC Address translation of "b3b3".
```
SWITCH -.
        |
        :---------> P2
        :---------> P3
        '---------> P4

.         |    __                        .------.                        __
.         |   [Ll] __________________ P1 | ---> | P2 __________________ [Ll]
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

And this is how the packet reaches HOST B. However, also notice that `the Switch did not learn the port that HOST B belongs to`.
Now, what if `HOST B` wants to reply to `HOST A`, it will send a packet back with `HOST A`'s MAC Address in the L2 Header.

```
.         |    __                        .------.                        __
.         |   [Ll] __________________ P1 | ---> | P2 __________________ [Ll]
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
.         |   [Ll] __________________ P1 | ---> | P2 __________________ [Ll]
.         |   ====     <--PACKET--    P3 | <--- | P4                    ====
MACHINE   |  HOST A                  /   '------'   \                  HOST B
MAC ADD   |   a2a2                  /                \                  b3b3
.         |    __                  /                  \                  __
.         |   [Ll] _______________/                    \_______________ [Ll]
.         |   ====                                                      ====
MACHINE   |  HOST C                                                    HOST D
MAC ADD   |   d7d7                                                      f1f1
```
-------------------------------------------------------------------------------------------------------------------

There is just one more special MAC address we need to get familiar with before we finish studying switches and it's :**FFFFFF:FFFFFF** that is the Broadcast address, this will make the switch always flood.


## ARP

**ARP**, or **A**ddress **R**esolution **P**rotocol is the protocol that is used to "translate" layer 3 IP Adresses to layer 2 MAC addresses, it is very simple. Technically, the protocol doesn't belong in the 3rd layer even though it does invlove IP addresses, this means that it is only possible to perform inside a LAN.

### THE WAY IT WORKS

```
MACHINE   |    HOST A                                                      HOST B
.         |    __   _                      .------.                        __   _
.         |   [Ll] |=| ________________ P1 | ---> | P2 __________________ [Ll] |=|
.         |   ====                         | <--- |                       ====
IP ADDR   |   10.1.1.22                    '------'                       10.1.1.37
MAC ADD   |   a2a2                                                        ?
```

Let's say I am `HOST A` and I want to send some information to `HOST B`, a ping for demonstration.
In order to perform any kind of communication on the internet the computer must know the IP Address of the endpoint it is trying to communicate with. So let's say I put it in manually.

The computer knows what the IP of the other machine is but not the MAC so it will use the ARP protocol.

```
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

MACHINE   |    HOST A                                                      HOST B
.         |    __   _    --ARP-REQ->       .------.                        __   _
.         |   [Ll] |=| ________________ P1 | ---> | P2 __________________ [Ll] |=|
.         |   ====                         | <--- |                       ====
IP ADDR   |   10.1.1.22                    '------'                       10.1.1.37
MAC ADD   |   a2a2                                                        ?

```

This is what the arp header looks like, if you look carefuly you might notice that the DST MAC is ffffff:ffffff which is the broadcasting address. So we can start to put an image together of what will happen.

`HOST A` sends a message asking any computer identifying with the IP address "10.1.1.37" to return a message with their MAC address to 10.1.1.22/a2a2, Ip address and MAC address respectively.

The switch will take the packet, duplicate it and send it on all ports except `P1`. Then HOST B will see that the packet is dedicated to them since it has their IP Address. and they will use the given ip and mac address to send the reply back saying that their mac address is "b3b3".

```
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

 HOST A                                                      HOST B
 __   _                      .------.      <-ARP-RESP--      __   _
[Ll] |=| ________________ P1 | ---> | P2 __________________ [Ll] |=|
====                         | <--- |                       ====
10.1.1.22                    '------'                       10.1.1.37
a2a2                                                        b3b3

```

And that is how devices find out mac addresses in a LAN network.



## Router

The router is a layer 3 network component used to move data between different networks, this process is called **routing** (The process of moving informationg inside of a network is called **switching**).