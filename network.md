# Network Basics

## NIC configuration

To show device and adress information:

    ip addr show eth0 //Can also put any other device instead of eth0.

To show statistics about network performance:

    ip -s link show eth0

## Network Services

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


## NetworkManager
To install NetworkManager:

    yum install NetworkManager

To show a list of all connections:

    nmcli con show // Use --active to only show active connections.

To see the details of a connection:

    nmcli con show "Wired connection 1"

To see a devices status and details:

    nmcli dev status

Or for a specific device:

    nmcli dev show enp2s0

To Creat a new connection:

    nmcli con add con-name "default" type ethernet ifname eth0

To disable an interface and prevent any autoconnection:

    nmcli dev disconnect DEVICENAME

To view different types of connections:

    nmcli con add help

To see modification key-value arguments of a connection:

    nmcli con show "Wired connection 1"

To modify an argument of a connection for example:

    nmcli con mod "Wired connection 1" connection.autoconnect no


## Static/Dynamic IP
To change the system to the static conneciton:

    nmcli con up "static"

To change back to the DHCP connection.

    nmcli con up "default"
## DHCP Client
1. DHCPDiscover, a new host sends a message to everyone. ignored by everyone but the DHCP server.
2. DHCPOffer, DHCP server sends an IP offer to the new host.
3. DHCPRequest, Ok is given from host to server.
4. DHCPACK, The server sends the IP address ,subnet mask ,default gateway, and the DNS server.

The DHCP server keeps a list of IP, MAC and Expiration date.
uses UDP Ports: client 68, server 67

Configuration in linux:

    vim /etc/network/interfaces

The __/etc/sysconfig/network-scripts/ifcfg-eth0__ file, should contain the following lines:

    DEVICE=eth0
    BOOTPROTO=dhcp
    ONBOOT=yes



## Route

To show routing information:

    ip route

To test connectivity:

    ping -c3 google.com

To trace the path to a remote host:

    traceroute google.com // The default is UDP. Use -I for ICMP and -T for TCP.




## Bond

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

          

## Virtual IP

## Netmask

A Netmask is a 32-bit "mask" used to divide an IP address into subnets and specify the network's available hosts. In a netmask, two bits are always automatically assigned. For example, in 255.255.225.0, "0" is the assigned network address. In 255.255.255.255, "255" is the assigned broadcast address. The 0 and 255 are always assigned and cannot be used.
