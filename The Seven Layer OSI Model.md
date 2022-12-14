## **The Seven Layer Model of OSI**

### **BRIEF HISTORY**

Computers nowadays are used all over the world, and the way they are connected is through the internet. Basically, a web of connections made up by all the machines in the world. However, it was not always like that...

Back in the 80s, computers used to communicate on their own dedicated networks, the format they used to transfer information with one another was different in each computer brand and that made communicating in-between them very difficult.

In order to solve that, a standard was proposed. It was delivered in 1984 and called the **OSI Model**. (The OSI model was updated in 1994 and it is the one we use today)

### **OVERVIEW**

The model is made up of 7 layers, each layer addressing a different aspect of networking and solving a problem that arises in that aspect. When a machine sends a network packet, it is then wrapped in the different layers according to the needs in a process called **encapsulation** and those are peeled off on the receiving machine by a process called **decapsulation**. (The first layer is not included in the _encapsulation_)

To better illustrate what I mean, I will take the classic example of sending a letter and represent that using the OSI model:

![How to send a letter using the OSI model](https://upload.wikimedia.org/wikipedia/commons/thumb/6/67/Rm-osi_parallel-HE.png/438px-Rm-osi_parallel-HE.png)

> How to send a letter using the OSI model

### **THE LAYERS**




#### **The First Layer, The Physical layer**

The physical layer isn't really a layer, it is also not involved in the encapsulation process. The Physical layer just consists of standards to physical networking components, like:

*   **Hubs**
*   **Network cables**
*   **Repeaters**
*   **Wi-Fi**

Simply put, the physical layer is just the components and wires that get the bits of information from point **A** to point **B** (Over a short geographic distance).




#### **The Second Layer, The Data-Link Layer**

The Data-Link layer solves the issues presented when sending information between physically connected computers. It serves the purpose of sending bits of information from one **NIC to another NIC**. A `NIC`: Network Interface Cards (Could also be a **Wi-Fi Access Card**), is a physical component in a machine that receives the information from the cable and into the machine.

**Each NIC** has a `MAC Addresses`. A MAC Address is a standard, "one of a kind", string of 48 bits represented by 12 _Hexadecimal_ characters (exmpl: A0-B1-C2-D3-E4-F5) that are exclusive to each NIC. We can then use that MAC Address to send information to another machine on the same network.

Another component in the 2nd layer is a `Switch`. A switch allows to connect multiple computers physically using network cables to creating a *local* network called "**LAN**"* (Local Area Network). A switch can also be used to group computers in the LAN to *sub*-networks called a "**VLAN**"s* that are "pseudo-isolated" from the rest of the computers connected to the switch, sending packets from one VLAN to another is still possible tough.

The 2nd layer also takes care of delivering a packet through routers using MAC Addresses, simply delivering to the next router **NIC MAC Address** until the target machine is reached.
This way of packet delivery is called **Hop-to-Hop**.


> LAN - A network of physically connected machines, limited by the need for cables and switches.
> 
> VLAN - A group of computers bound together in a switch port configuration under the same vlan tag that separates them from the rest of the computers/groups in the LAN.
```
             __             !__             !__             !__              __
            [C1] ---------- [R0] ---------- [R1] ---------- [R2] ---------- [C2]
            ====`o                                                          ====`o
MAC ADDR    a0-d2-e0        f0-d1-f2        a2-2f-3f        5f-2c-4d        a0-d2-e1
```

#### **The Third Layer, The Networking Layer**

The third layer serves the purpose of **End to End** packet delivery, by using a different addressing scheme then the one that layer 2 uses called `IP address`. An IP Address is made up of 32 bits represented as 4 octets, each 0-255, and that address is used to represent a computer in the wide web of machines connected to the internet. 

A component used that belongs to this layer is the `Router`, the router lets computers communicate outside of their networks in-directly, passing the packets though themselves and routing them to their end goal one router at a time.

```
             __             !__             !__             !__              __
            [C1] ---------- [R0] ---------- [R1] ---------- [R2] ---------- [C2]
            ====`o                                                          ====`o
MAC ADDR    a0-d2-e0        f0-d1-f2        a2-2f-3f        5f-2c-4d        a0-d2-e1
IP  ADDR    192.130.200.1                                                   192.130.44.21
```
**So why use both MAC and IP addressing?**
The simple answer is that they serve the same purpose in different methods that rely on eachother. Like we know, the 2nd layer work on a *hop by hop* system and the 3rd layer works on an *end to end* system. The IP address signifies the address of the end computer while the MAC address can be used to route the packet.

```
STEPS TO MOVE YOUR PACKET! (FROM C1 TO C2)
--------------------------------------------------------------------------------------------

    1.  Packet after leaving C1, on it's eay to the first router (R0).

    +---------------------------------------+
    |   DATA                                |
    |   011001010110000101110011011101000   |
    |   110010101110010                     |   
    +---------------------------------------+
    | SENDER IP ADDRESS     192.130.200.1   |
    | TARGET IP ADDRESS     192.130.44.21   |
    +---------------------------------------+
    | SENDER MAC ADDRESS    a0-d2-e0...     |
    | TARGET MAC ADDRESS    f0-d1-f2...     |
    +---------------------------------------+

    2.  Packet reaches R0 successfully, the router strips the layer 2 header.
        The router then attaches it's own layer 2 header to route the packet
        to the next router.

    +---------------------------------------+
    |   DATA                                |
    |   010001010110011101100111            |
    +---------------------------------------+
    | SENDER IP ADDRESS     192.130.200.1   |
    | TARGET IP ADDRESS     192.130.44.21   |
    +---------------------------------------+
    | SENDER MAC ADDRESS    f0-d1-f2...     |
    | TARGET MAC ADDRESS    a2-2f-3f...     |
    +---------------------------------------+

    And so it goes until the packet reaches IP address 192.130.44.21.

--------------------------------------------------------------------------------------------


```



#### **The Fourth Layer, The transport Layer**

The goal of the 4th layer, the transport layer, is to provide **Service to Service** communication. After tackling communication between different machines in and out of the LAN we need to then decide what service (application or computer service) recieves what information. After all, we wouldn't want our minecraft game to get our discord messages or youtube videos.

The way layer 4 sorts out the incoming `data streams` is with it's own addressing scheme called `transport protocols & ports`(similar to ports on a switch). And it works like that:
* TCP (Transmission Control Protocol)
    This protocol is used to transmit information over a stable connection between two endpoints (favors reliability). has 65535 ports.
    
    Features:
    * TCP Handshake.
    * Data-Loss recovery.
    * Flow control.

    Flaws:
    * Slower than UDP (Higher Round trip time).
    * Expensive to initiate.


* UDP (User Datagram Protocol)
    This protocol is used to send data without a pre-established connection very quickly and inexpensively (favors efficiency). has 65535 ports.
    
    Features:
    * Very fast.
    * Very simple.
    * 0 RTT.

    Flaws:
    * Not stable.
    * No data-loss recovery.
    * No Flow control.

Using the layer 4 header, an application on the local machine can decide what protocol it wants to use to send data on the 4th layer and what port to bind itself to. Every application that wants to recieve information from a remote location has to bind itself to a vacated port, by doing that it can make sure that it will recieve only the information that is sent by *hopefully* the server/client side of the same application and isolate it's own data stream from the other existing data streams that the machine is handling. Like a Minecraft server session, a Youtube video and a Discord messaging application.

To sum up the Packet structure up until now and how everything works we can equate it to a real life postal address.
First, the structure:
```
+-------------------------------+
|   DATA // The data being sent | 
|-------------------------------|
|   0100100001100101011011000   |
|   110110001101111             |
|                               |
'-------------------------------'
| LAYER 4 HEADER (TCP/UDP)      |
|-------------------------------|
|                               | 
| SENDER PORT           ...     |
| TARGET PORT           ...     |
'-------------------------------'
| LAYER 3 HEADER                |
|-------------------------------|
|                               | 
| SENDER IP ADDRESS     ...     |
| TARGET IP ADDRESS     ...     |
'-------------------------------'
| LAYER 2 HEADER                |
|-------------------------------|
|                               | 
| SENDER MAC ADDRESS    ...     |
| TARGET MAC ADDRESS    ...     |
'-------------------------------'
```




#### **The Fifth Layer, The Session Layer**

After addressing the network addressing problems and who is who and where we have layers 5, 6 and 7 which are can be freely intergrated by each application. 

Layer 5 just manages the session of the two endpoints:
* creating it at the beginning of communication 
* termintating it at the end of communication.
* Re-establish transport connection if it fails.
* Efficient diconnection handling using "checkpoints".
* Multiplex data across multiple transport connections. (HTTP 2.0 / HTTPS / QUIC)




#### **The Sixth Layer, The Presentation Layer**

The 6th layer is crucial, it is responsible for preparing the data to be ready for processing by the application layer. It's responsible for encrypting the data using encryption protocols, character set translation and data compression. Machines may use different encoding so this layer is responsible for encoding/decoding the information, if the two machines want to encrypt their sessions then this layer is also responsible for that. From the actual message encryption to establishing encryption algorithms and keys. And lastly this layer is responsible for compressing the data to improve efficiency.




#### **The Seventh Layer, The Application Layer**

The application layer contains application specific protocols that involve user input or output, it is actually the only layer that interacts with the user. A protocol that can be found in this layer is HTTP or SMTP.
