---
title:  "Basic Reconnaissance and Hacking"
author: Vince Jang
date: 2024-01-05 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /assets/img/google.jpg
render_with_liquid: false
---

## Basic Reconnaissance and Hacking

In this blog post, I will complete the configuration of the remaining virtual machines (VMs) and Defender network hosts that I didn`t finish discussing in the last post. Additionally, I will demonstrate the use of basic tools to exploit vulnerabilities in Defender network hosts.

## DHCP on the Attacker Router(R3)

It is normal for a small network to have dynamic IP leasing  

I used commands below to configure a DHCP pool for R3 to include a pool of addresses for the attacker network.  

ip dhcp pool < name >  
default-router < gateway address >  
domain-name < name of network >  
network < network address> < subnet mask  >  
ip dhcp excluded-address < ip address >  

And, Then I used the command `show ip dhcp` to confirm the pool is configured correctly.  

## Configure the IP address of the two defender machines. 

I have now configured the attacker network, and it`s time to set up the defender machines. Let`s start by configuring the CentOS machine. Use the command `nmtui` to configure the IP address, subnet mask, and default gateway.  

The last thing we need to configure is the Metasploitable VM. Since Metasploitable doesn`t have any GUI tools like `nmtui` in CentOS, we have to configure it by editing files and controlling services directly.  

Edit the file `/etc/network/interfaces`:  

Locate the lines identifying the network card`s configuration, which looks like this: `auto eth0, iface eth0 inet dhcp`  
Change those lines to:  

auto eth0  
iface eth0 inet static  
address < IP address >  
netmask < subnet mask >  
gateway < default gateway >  
Restart the networking service with the command `sudo /etc/init.d/networking restart`.  
Check the IP to ensure it is correctly configured. 

## Half way Testing

I am going to use ping(ICMP Protocol) to test network is connected.  

### Result

| **From** | **To** | **Protocol** | **IP Address** | **Ping Results** |
| ---- | ---- | ---- | ---- | ---- |
| Metasploitable VM | R2 Loopback (Representing Internet) | IPv4 | _8.8.8.8_ | _Yes_ |
|  | S1 VLAN | IPv4 | _192.168.1.2_ | _Yes_ |
|  | CentOS VM | IPv4 | _192.168.1.10_ | _Yes_ |
| Kali VM | R2 Loopback (Representing Internet) | IPv4 | _8.8.8.8_ | _Yes_ |
|  | Outside address of Defender Network Router | IPv4 | _200.10.1.1_ | _Yes_ |
|  | CentOS VM | IPv4 | _192.168.1.10_ | _Yes_ |
|  | Matasploitable VM | IPv4 | _192.168.1.11_ | _yes_ |
| CentOS VM <br><br> | R2 Loopback (Representing Internet) | IPv4 | _8.8.8.8_ | _Yes_ |
|  | Outside address of Attacker Network Router | IPv4 | _200.10.1.1_ | _yes_ |

## Reconnaissance

Use the Kali machine to discover information and footprint the defender network.

1. Use the `nmap` command to scan for all open port and services on the defender`s machine. `nmap -T4 -A 192.168.1.0/24`  
![images](/assets/img/Picture2.png)  
We can observe several open ports in the network, providing entry points to initiate penetration.

## Exploitation
Now we have lots of information regarding the target machines, including a list of services, versions and a list of users on the system. let`s begin a penetration test using some of this information with the Metasploit framework.
1. Open Metasploit on Kali Machine
2. after researching, I identified the service "UnrealIRCd" and "IRC" service is vulnerable to the 3281 backdoor.
3. We need to know what module we want to use. `searchsploit` is the command you can find a module that you want to use.
4. load the associated module for this backdoor with `use exploit/unix/irc/unreal_ircd_3281_backdoor`
5. Set the target of the attack using `set RHOST< targetIPaddress >` 
![images](/assets/img/Picture3.png)
6. Begin attack with `exploit`
7. Once the attack has succeeded, We should have completed root access to the machine, to check this use some command such as `date, whoami, hostname`  
### Result
![images](/assets/img/Picture4.1.png)
![images](/assets/img/Picture4.2.png)

Now, we can even type `reboot` to restart the target PC! This is Crazy..., as it implies that if a hacker successfully compromises your network, they can employ various techniques to infiltrate other machines within your network, such as remote PowerShell, SMIC, PSExec, scheduled tasks, etc.

## Exploitation 2

Samba(software used to make SMB(server message block)) version 3.0.20 through to 3.0.25rc3 had a vulnerability allowing attackers to excute arbitary commands by using script containing shell meta chracters at login. and I found out Metasploit has a framework containing these scripts for us to use!  

1. Open Kali, try to make a connection to the server as an anonymous user using the SMB protocol with `smbmap -H targetIPaddress`
2. it present with a list of enumerated shares and usernames readable to anonymous users, this include the tmp share which is also writable!
3. Now open Metasploit `msfconsole` 
4. Use commands `use exploit/multi/samba/usermap_script` , `set RHOST targetIPaddress`,`exploit`
5. After exploit has worked, use the command `id`. we now see we are root user on the system
6. now we are going to taking advantage of the open session to the other machine.
7. Switch metasploit modules to prebuilt password hash dumping module using:  
`use post/linux/gather/hashdump`  
`set SESSION 1`  
`exploit`
8. This should give me a full dump of the encrypted passwords, we can see this "/home/kali/.msf4/loot/192.168.76.128_linux.hashes_598149.txt"
9. now we will use John the Ripper to crack this hashed password. [what is John the Ripper](https://www.openwall.com/john/)
10.  Use the command `sudo john passwordFileName` to start a brute force attack on the password list. ex: `sudo john /home/kali/.msf4/loot/192.168.76.128_linux.hashes_598149.txt.`
11. Here We go. now John the Ripper break at least some of the password
![images](/assets/img/Picture5.png)



