---
title:  "Blue Team and Red Team Project 1. Cent OS Webserver Machine Hardening"
author: Vince Jang
date: 2024-04-09 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /assets/img/pic13.1.png
render_with_liquid: false
---

## Goal 

I am working on a group project that involves hardening three machines with pfSense Firewall and implementing as many security measures as possible. Then, we will swap the machines with another team and conduct penetration tests on each other's machines. Finally, we will compile a report detailing our findings
### Topology
![images](/assets/img/pic13.1.png)  

First of this project would be Hardening Webserver in CentOs Machine.  

let's see what services and what ports are open.  
![images](/assets/img/pic13.2.png)
![images](/assets/img/pic13.3.png)
![images](/assets/img/pic13.4.png)
![images](/assets/img/pic13.5.png)  
port and Services running.  

Before beginning, I will run a check and update the systems for any necessary patches and security updates  
![images](/assets/img/pic13.6.png)
![images](/assets/img/pic13.7.png)
![images](/assets/img/pic13.8.png)
![images](/assets/img/pic13.9.png)  
I have also updated the SSH version and adjusted the Firewalld configuration  
![images](/assets/img/pic13.10.png)
![images](/assets/img/pic13.11.png)
![images](/assets/img/pic13.12.png)  

After considering it, I've decided to use UFW for its simplicity. Additionally, I've installed auto-updates for an extra layer of security  
![images](/assets/img/pic13.13.png)
![images](/assets/img/pic13.14.png)
![images](/assets/img/pic13.15.png)
![images](/assets/img/pic13.16.png)  

### SSH Key Generation
I am going to enable SSH login only with private and public SSH keys. As a first step, I have created a folder where the keys will be saved  
![images](/assets/img/pic13.17.png)  
And I will generate the private key on a Windows machine. This key will be the only way to log in to the web server machine  
![images](/assets/img/pic13.18.png)
![images](/assets/img/pic13.19.png)  
The public key has been uploaded to the homer ./ssh directory. However, even after generating and uploading the key, SSH is still prompting for a password (TROUBLE!!!).

Troubleshooting:

I changed the permissions of the authorized_keys file to 600 so that SSH is able to log in with the key-based connection  
![images](/assets/img/pic13.20.png)  

And now I can log in without password.  
![images](/assets/img/pic13.21.png)  
Now I am going to disable password login. To do this, we need to make some changes to the configuration in the sshd config file  

![images](/assets/img/pic13.22.png)
![images](/assets/img/pic13.23.png)
![images](/assets/img/pic13.24.png)
![images](/assets/img/pic13.25.png)

### TEST
![images](/assets/img/pic13.26.png)  
Since I changed the port, it won't connect using port 22. The reason I changed the SSH port is because port 22 is well-known for SSH, so anyone aware of this port could attempt to connect. Making this change might enhance security.

I changed SSH port to 669 and it is now used as SSH port.  
![images](/assets/img/pic13.27.png)  
Now I want to see what port I want to close. And we have tons of port open...  

![images](/assets/img/pic13.28.png)
![images](/assets/img/pic13.29.png)
![images](/assets/img/pic13.30.png)
this is quick memo for open ports:

Port 631: internet printing protocol / DDoS attack possible. Close

Port 51413 : bittorrent client / since this webserver runs torrent services this se

Port 8080 http server

Port 111 network file system/ open network compyting remote procedure call. RPC

Port 5000 UPnP device service, accept incoming connections from other UPnP device

Port 9090 prometheus for serving its web UI and API, ensuring that there is no unrestricted inbound access to TCP port.

Port 8800 web server software in this case rutorrent UI is running

Port 631 submitting a print job or querying printer status / useless

Port 53 : DNS
Port 669 : SSH server./

And I am going to install unf firewall in centos  

Reference : [https://learnlinuxkkl.wordpress.com/install-and-setup-ufw-firewall-on-centos-8-rhel-8/](https://learnlinuxkkl.wordpress.com/install-and-setup-ufw-firewall-on-centos-8-rhel-8/)  

![images](/assets/img/pic13.31.png)
![images](/assets/img/pic13.32.png)
![images](/assets/img/pic13.33.png)
I have added rules to allow access only through port 669 (SSH). Now, this machine can only be accessed via SSH connection using the private key.  

Finally, to prevent hackers from knowing our web server's existence, I will block ICMP echo requests (ping) to the server  

![images](/assets/img/pic13.34.png)
![images](/assets/img/pic13.35.png)
Added this line.

![images](/assets/img/pic13.36.png)  
So now ICMP packet is blocked.