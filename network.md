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

#### Virtual IP

#### Netmask

#### Network+

A big LAN is divided intos smaller LANs called workgroups, for each department.

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
