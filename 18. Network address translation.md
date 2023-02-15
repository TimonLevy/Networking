# N.etwork A.ddress T.ranslation

Network Address Translation, or NAT for short, is a solution proposed to preserve the amount of IPv4 addresses in use. When IPv4 was first introduced it was unexpected that the internet and the use of IP addresses will grow to this magnitude.

There are 4,294,967,296 IPv4 addresses, but that is not nearly enough. So engineers devised a way to save them, by using public and private IPv4 addresses.

> Public IPv4 address, a reistered public ip that is necessary to connect to the internet.

> Private IPv4 address, an unregistered IP address to be used in your home network by the different machine.

However, if the devices in your home use private IP addresses and it is said that a public IP address is necessary to connect to the internet.. How will they do that exactly? You may choose to pay more money to your ISP (Internet Service Provider) and register public IPs for your devices but that would be a waste of money and IP addresses.

Instead, You can register only one IP address for your gateway to the internet, and then let it translate every private ip that goes through it to the single public IP address that it has. It work in reverse as well, NAT can translate public to private addresses.

## How does it do that?

The main goal is to translate an private IP address to a public IP address. Introducing the NAT table, it is a table to keep track of translations between private and public IP addresses, so when a packet with a private IP address gets to the router it will know what public address to use. And more importantly when a packet with the public ip arrives at the router it will know what machine to route it to.

There are multiple ways to translate addresses:

> **NAT Overload / PAT**
>
> This translation method is the most used, in it multiple private IP addresses are translated to a single public address on the router.
> The distinction between the connections is made by using the port numbers, since there are a lot of port numbers.

> **Dynamic NAT**
>
> In this translation, the router will have a pool of ore than one public addresses (which have to be bought). Afterwards, the router will translate one private IP address to one public IP address until all IP addresses in the pool are used. When there are no IPs left to use the local machines will have to wait for an address to become free.

> **Static NAT**
>
> In this translation the network administrator has to set the NAT tables by hand, mapping which address translates to which address including tcp ports.

> **Static PAT / Port Forwarding**
>
> In this translation, a singe public IP address is translated to a single private IP address however specific IP and port combinations can be translated to other specific private IP and port combinations. Like:
> 
> | Private____________________ | Public_______________________ |
> | ----                        | ----                          |
> | 10.0.1.1                    | 192.168.142.22                |
> | 10.0.1.25:80                | 192.168.142.22:80             |

## Encouters with NAT in the wild

As attackers we will sometimes wants to perform some actions on a network from the outside, maybe because we still have not gained access to it. In that case we will have to use it's public IP/IPs in order to interact with it, so what do we do? Since those addresses don't necessarily map to a single machine we wil perform a port scan on that address. See what possible attack vectors that address allows.

From then we will act upon the findings or perform some more recon based on them.