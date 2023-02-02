# IP FRAGMENTATION AND WEAKNESSES

IP Fragmentation is the process of breaking down and Internet Protocol packet to multiple smaller packets called "fragments", that is done when a packet is sent from a network with high MTU (Maximum Tranmission Unit) to a network with a lower MTU.

### How Is It Done

> #### Fragmentation
>
> First, if the `Don't fragment` flag is on in the IP header the pacet will not be fragmented. Else, N new ip packets are created.  Then the IP header is duplicated from the original packet to the new smaller packets and the data is devided into those packets equally in bytes. The IP header also undergoes some changes:
> * `more-fragments` flag is turned on for all of the packets, for the last fragments it is set to the value from the header of the original packet.
> * `Identification` the packets are given an identification number to state that they belong to the same original packet.
> * `fragment offset` the fragment offset is set to 0 on the *first fragment*, and the rest are set to amount of data offset in 8 byte blocks (offset of 400 bytes / 8 bytes = 50).
> * `total length` is set to the length of the new packet.

> #### Reassembly
>
> The way the packet is reconstituted is first by finding the fragments that belong to it. There are 4 things that need to match in order to tie it to the same packet: Destination IP, Source IP, Protocol & Identification number. When discovering a new packet a buffer is created for it, then the arriving packets are put inside. The buffer is a list of "hole"s, a hole is a continous region inside the buffer whose bytes are bank and not yet occupied by a fragment.
>
> The hole object has a hole.first and a hole.last property, both containing the first and last byte of the hole. At first the entire buffer is one big hole (hole.last will be a really large number).
>
> When a fragment arrives it is checked against the holes in the list, if it fills the start\end\middle\part of any hole in the list. Then it is put inside of the buffer and the holes are changed correspondingly. That is done for each fragment until the packet is done (A fragment with the `More Fragments (MF)` turned off is recieved and all of the data from 0 to that fragment is already received).

### How It Can Be Exploited

Packet fragmentation can be exploited and that is due to the fact that packet fragment reassembly takes resources from the target computers. When reassembling a packet the computer has to open a new buffer for it, and that is a vulnurability that can be taken advantage of by attackers.

> #### UDP & ICMP Fragantation attacks
>
> These attacks are using protocol that are stateless, create "bad" packets, fragment them and send them over to the victim. The fragments cannot be reassembled on the victim machine since they are not valid and thus when sent in a volume, ovewhelm the target machine and deplete their resource.