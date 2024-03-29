In my summary of the `7 Layer OSI Model` I talked about 3 techniques that are uses to route and forward  packets through the internet, `Hop by Hop`, `End to End`, and `Service to Service`. These 3 methods allow communication between machines. Let's break it down:

### Switch

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
```diff
- (I think the sentence above is incomplete)
```
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
 HOST A                                                      HOST B
 __   _                      .------.                        __   _
[Ll] |=| ________________ P1 | ---> | P2 __________________ [Ll] |=|
==== '-'                     | <--- |                       ==== '-'
10.1.1.22                    '------'                       10.1.1.37
a2a2                                                        ?
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

 HOST A                                                      HOST B
 __   _    --ARP-REQ->       .------.                        __   _
[Ll] |=| ________________ P1 | ---> | P2 __________________ [Ll] |=|
==== '-'                     | <--- |                       ==== '-'
10.1.1.22                    '------'                       10.1.1.37
a2a2                                                        ?
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
==== '-'                     | <--- |                       ==== '-'
10.1.1.22                    '------'                       10.1.1.37
a2a2                                                        b3b3
```

And that is how devices find out mac addresses in a LAN network.



## Router

The router is a layer 3 network component used to move data between different networks, this process is called **routing** (The process of moving informationg inside of a network is called **switching**). To better explain what exactly constitues a router and how it is different than a host, here is the [RC 2460](https://www.rfc-editor.org/rfc/rfc2460) definition of a router (The RFC is about IPv6 but the definition of a router applies to IPv4 as well):

>   node        - a device that implements IPv6.
>
>   router      - a node that forwards IPv6 packets not explicitly
>                 addressed to itself.
>
>   host        - any node that is not a router.

```diff
- (Wy IPv6?)
```

This basically means that **a host would drop a packet that is not intended for it**, but a **router** will try to **route it forward**.

Since routers are connected to multiple networks at once and must maintain the ability to interact with all of them (to route packets), **they must have an IP + MAC address on all networks they participate in AND must maintain a map of all of those networks.** This map is called a `Routing Table`. When a router gets a packet destined to a network it doesn't know how to route to, i.e it doesn't exist in it's routing table, it will drop it, otherwise it will send it to the next node in the chain to the target address. 
```diff
- (If the packet is dropped or forwarded is dependent on configuration)
```

```
+--------------10.0.55.x/24-------------+              +--------------10.0.44.x/24-------------+                +--10.0.66.x/24--+
|            __   _                     |              |                     __   _            |                |                |
|       .3  [Ll] |=| ______             |              |             ______ [Ll] |=|   .6      |                |                |
|     f5f5  ==== '-'       \            |              |            /       ==== '-' a2a2      |                |                |
|                           \           | eth1    eth0 |           /                           |   eth1    eth0 |                |
|            __   _          \ +----+   |    +----+    |   +----+ /          __   _            |      +----+    |                |
|       .4  [Ll] |=| _________\| S1 |------> | R1 | <------| S2 |/_________ [Ll] |=|  .20      | .--> | R2 | <----      ...      |
|     e1e1  ==== '-'          /+----+   | .2 +----+ .1 |   +----+\          ==== '-' b3b3      | | .2 +----+ .1 |                |
|                            /          | c444    fff2 |     |    \                            | | dd22    ee34 |                |
|            __   _         /           |  ^        ^  |     |     \         __   _            | |              |                |
|       .2  [Ll] |=| ______/            |  |        |  |     |      \______ [Ll] |=|   .2      | |              |                |
|     22ec  ==== '-'                    |  '        '  |     |              ==== '-' d4d4      | |              |                |
|                                       |   \      /   |     |                                 | |              |                |
+---------------------------------------+    \    /    +-----|---------------------------------+ |              +----------------+
.                                             \  /           |___________________________________|
.                                              \/
.                                   Router interfaces IP address
.                                   And MAC.
```

```diff
- (Why is S2 connected to 2 roters?)
```

Now, there are three types of routing paths that can be written into a routing tables. 

First, the `Directly Connected`, the networks that interface directly into the router are written into the **Routing Table** when configured on the router. For example Router 1 (R1) will have two directly connected networks, 10.0.55.0 and 10.0.44.0 and they will be written like that:

| **R1 ROUTING TABLE** | TYPE  | NETWORK       | INTERFACE   |   |   | **R2 ROUTING TABLE** | TYPE  | NETWORK       | INTERFACE   |
| -------------------- | ----- | ------------- | ----------- | - | - | -------------------- | ----- | ------------- | ----------- |
| 1                    | DC    | 10.0.44.0     | Eth0        |   |   | 1                    | DC    | 10.0.66.0     | Eth0        |
| 2                    | DC    | 10.0.55.0     | Eth1        |   |   | 2                    | DC    | 10.0.44.0     | Eth1        |

Second, the `Static Routing`. If Router 1 (R1) was to get a packet destined to 10.0.66.50 it would start searching the Routing Table and won't find the netwrok since it's not directly connected, meaning it would drop it. That is what static routing is for, basically static routes are manually entered networking routes. Supposedly, if I want to get a packet from the network 10.0.55.0 to the network 10.0.66.0 which is connected to Router 2 (R2), I would have to go through it. To get to Router 2 I need to have some sort of known interface with it, which in this case is network 10.0.44.0, where the router R2 posesses the address of 10.0.44.2. So if I want to put that in the Routing Table I would do:

| **R1 ROUTING TABLE** | TYPE     | NETWORK       | INTERFACE   |   |   | **R2 ROUTING TABLE** | TYPE     | NETWORK       | INTERFACE   |
| -------------------- | -------- | ------------- | ----------- | - | - | -------------------- | -------- | ------------- | ----------- |
| 1                    | DC       | 10.0.44.0     | Eth0        |   |   | 1                    | DC       | 10.0.66.0     | Eth0        |
| 2                    | DC       | 10.0.55.0     | Eth1        |   |   | 2                    | DC       | 10.0.44.0     | Eth1        |
| 3                    | Static   | 10.0.66.0     | 10.0.44.2   |   |   | 3                    | Static   | 10.0.55.0     | 10.0.44.1   |
*I also added 10.0.44.0 to R2's routing table*

```diff
- (Please explain to me this table in person)
```

Then when the packet reaches Router 2 the other router (R2) will route it to it's DC (Directly connected) network.

Lastly, `Dynamic Routing`. When setting a Router to `Dynamic Routing` it will occasionally share it's known networks to other routers connected to it, Meaning that if we take the Routing tables from the first routing option (The ones without static routes) and enable Dynamic Routing on both rouers they would share their routing tables. When a router shares their routing table information it will add all the network it is not familiar with. So if we take the routing tables from example 1 and enable `Dynamic Routing` this is what will happen:

1. The routers' routing tables before sharing their networks. 
   
| **R1 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |   |   | **R2 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |
| -------------------- | --------- | ------------- | ----------- | - | - | -------------------- | -----     | ------------- | ----------- |
| 1                    | DC        | 10.0.44.0     | Eth0        |   |   | 1                    | DC        | 10.0.66.0     | Eth0        |
| 2                    | DC        | 10.0.55.0     | Eth1        |   |   | 2                    | DC        | 10.0.44.0     | Eth1        |
| -                    | -         | -             | -           |   |   | -                    | -         | -             | -           |

2. R1 shares t's known networks, R2 recognizes 10.0.44.0 so it dismisses it but does not recognize 10.0.55.0 so it adds it to it's routing table from the addess it got it from.

| **R1 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |   |   | **R2 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |
| -------------------- | --------- | ------------- | ----------- | - | - | -------------------- | -----     | ------------- | ----------- |
| 1                    | DC        | 10.0.44.0     | Eth0        |   |   | 1                    | DC        | 10.0.66.0     | Eth0        |
| 2                    | DC        | 10.0.55.0     | Eth1        |   |   | 2                    | DC        | 10.0.44.0     | Eth1        |
| -                    | -         | -             | -           |   |   | 3                    | Dynamic   | 10.0.55.0     | 10.0.44.1   |

3. R2 shares t's known networks, R1 recognizes 10.0.44.0 so it dismisses it but does not recognize 10.0.66.0 so it adds it to it's routing table from the addess it got it from.

| **R1 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |   |   | **R2 ROUTING TABLE** | TYPE      | NETWORK       | INTERFACE   |
| -------------------- | --------- | ------------- | ----------- | - | - | -------------------- | -----     | ------------- | ----------- |
| 1                    | DC        | 10.0.44.0     | Eth0        |   |   | 1                    | DC        | 10.0.66.0     | Eth0        |
| 2                    | DC        | 10.0.55.0     | Eth1        |   |   | 2                    | DC        | 10.0.44.0     | Eth1        |
| Dynamic              | Dynamic   | 10.0.66.0     | 10.0.44.2   |   |   | 3                    | Dynamic   | 10.0.55.0     | 10.0.44.1   |

Hooray, now, without any intervention, networks 10.0.55.0 and 10.0.66.0 can communicate!