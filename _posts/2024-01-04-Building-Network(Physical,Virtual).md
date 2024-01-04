---
title:  "Building Physical Network"
author: Vince Jang
date: 2024-01-04 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /assets/img/Building-network-topology.png
render_with_liquid: false
---

# Building Physical Network

Firstly, I would like to express my appreciation to NMTAFE and the excellent lecturer, Alan, for providing me with the opportunity to undertake this project involving hands-on experience with physical network components such as routers and switches.

Thanks to this project, I have gained a clear understanding of how switches and routers function, and I've learned the necessary commands for configuring a small network.

In addition, I plan to utilize this network to practice and enhance my basic hacking skills, including activities such as port scanning, DDoS attacks, and exploiting network vulnerabilities using tools like Samba and Metasploit.

## The Goal

The Goal of building Physical Network

* To understand Basic networking and security concepts
* Implement and demonstrate the function and operation of key networking devices
* Implement the components of network security laboratory and testing environment
* Identify typical cyber security application layer testing methodologies and tools
* Use networking security testing methodologies, tools, and commands

## Components

for this project, I will be building a physical network with componets including:

* installing Kali Linux image for later project.
* 3 X Cisco IOS-XE router
* 2 x Cisco swtich
* 2 x Hupervisor machines
* VM (Kali Linux, Metasploitable VM, CentOS VM)
* RJ 45 cables

## Topology

![images](/assets/img/Building-network-topology.png)

## Basic Device Configuration


| ---- | ---- | ---- | ---- |
| **Device** | **Hostname** | **Interface** | **IP configuration** |
| **R1** | EXT-RTR | Gig 0/0/0 | 192.168.1.1/24 |
| Gig 0/0/1 | 200.10.5.1/16 |
| **R2** | R2 | Gig 0/0/0 | 100.50.10.1/16 |
|  |  | Gig 0/0/1 | 200.10.1.1/16 |
|  |  | Lo0 | 8.8.8.8/32 |
| **R3** | BAD-GUY-RTR | Gig 0/0/1 | 100.50.20.1/16 |
|  |  |Gig 0/0/0 | 192.168.2.1/24 |
| **S1** | S1 | VLAN1 | 192.168.1.2/24 |
| **S2** | S2 | VLAN1 | 192.168.2.2/24 |
| **PC-1** |  |  |  |
| **PC-2** |  |  |  |
| **CentOS VM** |  | eth0 | 192.168.1.10/24 |
| **Metasploit VM** |  | eth0 | 192.168.1.11/24 |
| **Kali VM** |  | eth0 | DHCP |

## Basic Device Configuration Cisco Commands.

#### Switch - Ex: S1

no ip domain-lookup  
hostname S1  
enable secret class  
interface vlan 1  
ip address 192.168.1.2 255.255.255.0  
description limit access for remote users.  
no shut  
exit  
ip default-gateway 192.168.1.1  
line con 0  
password cisco  
line vty 0 4  
password cisco  
login  
line vty 5 15  
password cisco  
login  
end  

#### Router - Ex: R1

hostname EXT-RTR  
enable secret class  
no ip domain lookup  
ip domain name mayojang.com  
username admin password cisco  
interface GigabitEthernet0/0/0  
description connect to S1  
ip address 192.168.1.1 255.255.255.0  
no shut  
interface GigabitEthernet0/0/1  
description connect to R2  
ip address 200.10.5.1 255.255.0.0  
no shut  
ip route 0.0.0.0 0.0.0.0 200.10.1.1  
ip ssh version 2  
show ip ssh  
crypto key generate rsa   
2048  
line con 0  
 password cisco  
 login  
line vty 0 4  
 login  
 transport input ssh  
line vty 5 15  
 login  
 transport input ssh  
end  

## Trouble Shooting

after basic configuration I faced a couple of issues
1. Ping from R1 to R2 Loopbakc IP doesn't work.
2. ping from R3 to R2 Loopback IP doesn't work.
3. Ping between R1 and R3 doesn't work.

#### What is problem?

I used Bottom-up trouble shooting method, commands used: ping, show ip route, show interface xxx.

and I found out R1 and R3 don't have a route to the packet destination so they dropped the packet. to address this, I set default route(0.0.0.0/0) as the next destination which is the least specific route possible for internal network and the internet. and also static routes configured on R2 router for R1 and R3.

R2 couldn't figure out how to send packet to the VMs in the defender and attacker networks becuase it didn't know where these network were. To address this I configured static routes on R2 using
(ip route 192.168.1.0 255.255.255.0 200.10.5.1
ip route 192.168.2.0 255.255.255.0 100.50.20.1) commands.

After all I used Traceroute test
results:![images](/assets/img/Picture1.png)

## Thank you



