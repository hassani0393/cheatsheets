### Network Basics
____

#### NIC configuration

ifconfig : interface configuration, prints a list of network interfaces with their configurations.

netstat : network statistics, all connections established currently in the host. -t : tcp connection  -l : listening sockets(open ports) 
          -n : ip adresses as numbers(do not resolve host names.)
ip link : shows status of available network interfaces

loopback represents the host itself 127.0.0.1 localhost

ip addr : shows the ip adress configuration of network interfaces

route 
ifdown

#### Network Services

The file /etc/services lists network services.

| Port Number | Protocol | Service |
| --- | --- | --- |
| 20(data), 21(control) | tcp, udp | FTP |
| 22 | tcp | SSH (Secure Shell) |
| 23 | tcp | telnet |
| 25 | tcp | SMTP (Simple Mail Transfer Protocol) |
| 53 | udp(primary), tcp | DNS (Domain Name Server) |
| 80 | tcp | HTTP (Hyper Text Transfer Protoclol, web sites) |
| 110 | tcp | POP (Post Office Protocol) |
| 123 | udp | NTP (Network Time Protocol) |
| 139 | tcp, udp | NETBIOS (for Samba shares) |
| 143 | tcp | IMAP (Interim Mail Access Protocol) |
| 161 | tcp, udp | SNMP (Simple Network Management Protocol) |
| 162 | tcp, udp | SNMP Trap |
| 389 | tcp | LDAP (Lightweight Directory Access Protocol) |
| 443 | tcp | HTTPS (Secure HTTP) |
| 465 | tcp | SMTP Secure |
| 514 | tcp, udp | Syslog |
| 636 | tcp | LDAPS (Secure LDAP) |
| 993 | tcp | IMAPS (Secure IMAP) |
| 995 | tcp | POP3S (Secure POP) |


#### NetworkManager

#### Static/Dynamic IP

#### DHCP Client

#### Route

#### Bond

To activate multiple network cards behind the same ip address, this is called bonding.

We start with ifconfig -a to get a list of all the network cards on our system.
ifconfig -a | grep Ethernet
eth0      Link encap:Ethernet  HWaddr 08:00:27:DD:0D:5C  
eth1      Link encap:Ethernet  HWaddr 08:00:27:DA:C1:49  
eth2      Link encap:Ethernet  HWaddr 08:00:27:40:03:3B

To bond eth1 and eth2:

We will name our bond bond0 and add this entry to modprobe so the kernel can load the bonding module when we bring the interface up.
cat /etc/modprobe.d/bonding.conf 
alias bond0 bonding

Then we create /etc/sysconfig/network-scripts/ifcfg-bond0 to configure our bond0 interface.

pwd
/etc/sysconfig/network-scripts
cat ifcfg-bond0 
DEVICE=bond0
IPADDR=192.168.1.199
NETMASK=255.255.255.0
ONBOOT=yes
BOOTPROTO=none
USERCTL=no

Next we create two files, one for each network card that we will use as slave in bond0.

#cat ifcfg-eth1
DEVICE=eth1
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
#cat ifcfg-eth2
DEVICE=eth2
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no

Finally we bring the interface up with ifup bond0.

#ifup bond0
#ifconfig bond0
bond0     Link encap:Ethernet  HWaddr 08:00:27:DA:C1:49  
          inet addr:192.168.1.199  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:feda:c149/64 Scope:Link
          UP BROADCAST RUNNING MASTER MULTICAST  MTU:1500  Metric:1
          RX packets:251 errors:0 dropped:0 overruns:0 frame:0
          TX packets:21 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:39852 (38.9 KiB)  TX bytes:1070 (1.0 KiB)

The bond should also be visible in /proc/net/bonding.

cat /proc/net/bonding/bond0 
Ethernet Channel Bonding Driver: v3.5.0 (November 4, 2008)

Bonding Mode: load balancing (round-robin)
MII Status: up
MII Polling Interval (ms): 0
Up Delay (ms): 0
Down Delay (ms): 0

Slave Interface: eth1
MII Status: up
Link Failure Count: 0
Permanent HW addr: 08:00:27:da:c1:49

Slave Interface: eth2
MII Status: up
Link Failure Count: 0
Permanent HW addr: 08:00:27:40:03:3b

To have more than one ip address on the same network card, we call this binding ip addresses.

To bind extra IP addresses: 

cat /etc/sysconfig/network-scripts/ifcfg-eth0:0             //Last zero can be anything else.
DEVICE="eth0:0"
IPADDR="192.168.1.133"
cat /etc/sysconfig/network-scripts/ifcfg-eth0:1
DEVICE="eth0:0"
IPADDR="192.168.1.142"

To up and down a virtual network interface(use ifdown to down.):

ifup eth0:0
ifconfig | grep 'inet '
inet addr:192.168.1.99  Bcast:192.168.1.255  Mask:255.255.255.0
inet addr:192.168.1.133  Bcast:192.168.1.255  Mask:255.255.255.0
inet addr:127.0.0.1  Mask:255.0.0.0

ifup eth0:1
ifconfig | grep 'inet '
          inet addr:192.168.1.99  Bcast:192.168.1.255  Mask:255.255.255.0
          inet addr:192.168.1.133  Bcast:192.168.1.255  Mask:255.255.255.0
          inet addr:192.168.1.142  Bcast:192.168.1.255  Mask:255.255.255.0
          inet addr:127.0.0.1  Mask:255.0.0.0
          
To verify extra ip-adresses use ifconfig.

          

#### Virtual IP

#### Netmask

#### Network+

A big LAN is divided into smaller LANs called workgroups, for each department.

A router is used to connect LANs.

| Common Dedicated servers | Duty |
| --- | --- |
| File Server | Stores and dispenses files |
| Mail Server | The network's post office; handles email functions |
| Print Server | Manages printers on the network |
| Web Server | Manages web-based activities by running Hypertext Transfer Protocol (HTTP) for storing web content and accessing web pages |
| Application Server | Manages network applications |
| Proxy Server | Handles tasks in the place of other machines on the network, particularly an internet connection. |

In TCP/IP(Transmission Control Protocol/Internet Protocol) speak, a host is any network device with an IP address. including servers and workstations.

In an internetwork, hosts on a LAN use hardware addresses(MAC) to communicate with other hosts on the LAN, but logical Addresses(IP adresses) to communicate with hosts on a different LAN on the other side of the router.

Every connection into a router is a different logical network.

WAN protocol MPLS, is a switching mechanism that imposes labels (numbers) to data and then uses those labels to forward data when it arrives at the MPLS network. 

Common Physical Network Topologies: Bus, Star, Ring, Mesh, Point-to-point, Point-to-multipoint, Hybrid

Network Backbone, Network segment

CAN : campus area network, s network that encompasses several building where data, services and connectivity to the outside
world is provided to those who work in the corporate office or headquarters.

SAN : Storage area network, high capacity storage devices that are connected by a high speed private network, separate from the LAN, using a storage specific switch. Typically fiber networks.

Binding is the process of having communication processes relating and understanding eachother.

mnemonic for remembering OSI's seven layers : " Please Do Not Throw Susage Pizza Away. "

The Application Layer: The spot where users acually communicate or interact with the computer. typically through application processes, interfaces, or APIs that connect the application to the OS. Doesn't include the applications themselves but comes to play when an application needs to access remote resources.

The Presentation Layer: Presents data to the application layer and is responsible for data translation and code formatting. Makes sure that data transfered by the application layer from one system can be read and understood by the application layer on another system, by providing neccessary translation services. Tasks like data compression, decompression, encryption and decryption happen at this layer.

The Session Layer: responsible for setting up, managing and then tearing down sessions between presentation layer entities. Also provides dialogue control between devices, or nodes. Keeps each application's data seperate from another's. ex. multiple chrome tabs at the same time

The Transport layer: Segments and reassembles data into a data stream. provides end-to-end data transport services and can establish a logical connection between the sending host and destination host on an internetwork. must understand connection-oriented(reliable) protocol of the transport layer. buffering flood of datagrams and flood-control system are part of the transport layer.

connection-oriented communication: Sender's TCP proccess contacts the reciever's TCP process to establish a connection. The resulting connection is called a virtual circuit. They agree on the amount of information that will be sent in either direction. The recivier's TCP sends back an acknowledgment. The virual circuit setup is called __overhead__.

Windowing: a window is the quantity of data segments(bytes) that the transmitting machine is allowed to send without receiving an acknowledgment.

Positive Acknowledgment with Retransmission makes sure that data won't be lost or duplicated. 

TCP is connection-oriented and UDP is connectionless, and the choice is up to the application developer. The transport layer is capable of both.

These network devices operate at all seven layers of the OSI model: Network management stations (NMS), Web and application servers, Gateways (not default gateways), Network hosts

These devices operate primarily at the Physical layer: NICs, Transceivers, Repeaters, Hubs

The Network Layer: manages logical device addressing, tracks the location of devices on the network and detemines the best way to move data.Transports traffic between devices that aren't locally attached. Routers are layer 3 devices, specified at the Network layer and provide the routing services within an internetwork.

The Data Link Layer: Provides the physical transmission of the data and handles error notification, network topology, and flow control. That messages are delivered to the proper device on LAN using hardware(MAC) addresses and translates messages from the Network layer into bits for the physical layer to transmit.
Message to data frame, adds a customized header containing the destination and source hardware addresses.

