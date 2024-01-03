---
title:  "Building CyberSecurity HomeLab"
author: Vince Jang
date: 2023-10-17 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /dd/to/image
render_with_liquid: false
---


# Cyber Security HomeLab Project

It is important to apply what you have learned in class and through self-study to hands-on experiences. Without real-world application, it can be challenging to understand how concepts are actually implemented in the industry. 

To gain a fundamental understanding of how different components work together, I engage in real-world applications. These applications can take various forms, including hands-on projects and virtualized environments where I test new approaches to problem-solving.

## The Goal

The goal of a homelab is to understand the process of installing, configuring, and optimizing I.T. infrastructure at a relatively small scale so one can apply similar processes to a real-world business or enterprise network.

Additionally, I want to assist people who have just started their journey in cybersecurity, like myself, in building their own homelab. This will enable them to develop their skills for real-world industries.

## Cybersecurity Homelab Project

I have decided to name this project the “V Cybersecurity Homelab Project” using my initials.

for this cybersecurity homelab projeck, I will be building and simulating a business network with different components including:

* installing Window 10 image
setting up VMware workstation pro player
* configuring and working with Active Directory
* setting up and connecting Window 10 desktop and Linux VM images to AD
* configuring and securing remote Desktop protocol(RDP)
setting up and working with the open source PF sense firewall
* building a small-scale VPN server(OpenVPN)
* configuring a vulnerability scanner(Nessus) and  simulating vulnerability scans
* configuring a system information Event Management System(SIEM) with splunk and logging events 
* more


to begin the Cybersecurity Homelab project, I have first created a network topology of what I want my environment to look like after completion( this could be subject to changes as I continue throughout the project). Creating this little network topology has helped me get a general understanding of what I am trying to do with this particular setup.

## Network Topology

![images](assets/images/dd.png)

#### <center> Topology Breakdown <center>


### Main PC

#### PC spec
* CPU : AMD Ryzen 7 2700x 8-core
* GRAPHIC CARD : NVIDIA GeForce GTX 1060 3GB
* Mainboard : ASUSTeK PRIME X 470-PRO
* RAM: G.Skill Series 32GB DDR4
* Hard Drive : Samsung SSD 4TB
* OS : Window 10 Pro

#### Installing Ubuntu Image

I will be installing a version of Ubuntu Linux 20.04.3 x 64 Desktop. if you don’t have image you can download base version of Ubuntu Linux from Ubuntu Website.
[link](https://ubuntu.com/download/desktop)

Setting up VMware Workstation 17 Player

VMware workstation 17 Player is free for personal user. I will be using workstatin to create, configure and maintain my VMs.[https://customerconnect.vmware.com/en/downloads/info/slug/desktop_end_user_computing/vmware_workstation_player/17_0](https://customerconnect.vmware.com/en/downloads/info/slug/desktop_end_user_computing/vmware_workstation_player/17_0)


### Configuring and working with Active Directory

My PCs will be managed by AD. I will be setting up an AD environment. I will be using “The Cyber Mentor”s “How to Build an Active Directory Hacking Lab” youtube tutorial.[https://www.youtube.com/watch?v=xftEuVQ7kY0]
(https://www.youtube.com/watch?v=xftEuVQ7kY0)

### Securing Home Network with PFsense Firewall
setting up and working with the open source PFsense Firewall

providing network segmentation for my home lab is important. To secure home lab environment, I will be using the open source(IT IS FREE!!) PF sense firewall / router.
Download is available on the official website and I will be directly installing pfsense in VMware. I have linked a blog post to outline the process of creating a pfsense VM in Vmware Workstatin which I exactly followed.
[https://www.pfsense.org/download/](https://www.pfsense.org/download/)
[https://www.vgemba.net/vmware/pfSense-VMware-Workstation/](https://www.vgemba.net/vmware/pfSense-VMware-Workstation/)

### Configuring and Securing Remote Desktop Protocol(RDP)

Remote Desktop protocol(RDP) is a very popular service used in many enterprise networks to enable remote access and administration for computers and servers. Because of its popularity, RDP is often subject to attack, meaning it’s critical to take necessary measures to ensure security. I will be configuring a RDP server as well as taking the necessary precaution to secure the server. The link introduces methods to secure RDP.[https://security.berkeley.edu/education-awareness/best-practices-how-tos/system-application-security/securing-remote-desktop-rdp](https://security.berkeley.edu/education-awareness/best-practices-how-tos/system-application-security/securing-remote-desktop-rdp)

### Setting up and working with Palo Alto PAN-OS 11
I have researched how to set up a homelab and found that it's important to put the RDP server behind its very own firewall for security. This also gives me the opportunity to configure and maintain a different type of firewall. I will be using a virtual image of PAN-OS 11 and it provides 30days free trial.
[https://www.paloaltonetworks.com/products/products-a-z](https://www.paloaltonetworks.com/products/products-a-z)

### Security Components

#### building a small-scale VPN server

VPN servers are typically seen in enterprise networks where employees must remotely access company information. In this home lab, I will configure a small-scale VPN server using OpenVPN. The link includes information pertaining to setting up OpenVPN with Vmware solutions.
[https://openvpn.net/vpn-server-resources/deploying-the-access-server-appliance-on-vmware-esxi/](https://openvpn.net/vpn-server-resources/deploying-the-access-server-appliance-on-vmware-esxi/)

### Configuring a vulnerability scanner(Nessus) and simulation vulnerability scans

Vulnerability scanners offer a means of identifying and actively managing known vulnerabilities. Tenable Nessus is a popular vulnerability scanner used in all kinds of corporate network. Nessus Essentials is a free vulnerability scanner you can use to scan up to 16 IPs. Since this homelab will be quite small, I will be using Nessus Essentials as my vulnerability scanner.
[https://www.tenable.com/products/nessus/nessus-essentials](https://www.tenable.com/products/nessus/nessus-essentials)

### Configuring a System information Event Management System(SIEM) with Splunk and logging events.

System information Event Management System(SIEMs) are used to correlate and congregate logged events and alerts into meaningful and actionable data so an organization can prioritize critical alerts. Splunk is a well-known SIEM tool within the industry. Splunk provides free trial, I will be using this as my SIEM
[https://www.splunk.com/en_us/download/splunk-enterprise.html](https://www.splunk.com/en_us/download/splunk-enterprise.html)








## <center> SOURCES <center>

How to set up a homelab from hardware to firewall: [https://opensource.com/article/19/3/home-lab](https://opensource.com/article/19/3/home-lab)

Download Windows 10: [https://www.microsoft.com/en-us/software-download/windows10](https://www.microsoft.com/en-us/software-download/windows10)

Download VMware Workstation 15 Player: [https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html](https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html)

The Cyber Mentor - How to Build an Active Directory Hacking Lab: [https://www.youtube.com/channel/UC0ArlFuFYMpEewyRBzdLHiw](https://www.youtube.com/channel/UC0ArlFuFYMpEewyRBzdLHiw)

Download Ubuntu Desktop: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

Download a Windows 10 virtual environment: [https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/](https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/)

pfSense Download Center: [https://www.pfsense.org/download/](https://www.pfsense.org/download/)

pfSense VMware Workstation Introduction: [https://www.vgemba.net/vmware/pfSense-VMware-Workstation/](https://www.vgemba.net/vmware/pfSense-VMware-Workstation/)

How to enable Remote Desktop Protocol on Windows Server: [https://documentation.online.net/en/dedicated-server/rescue/enable-windows-rdp](https://documentation.online.net/en/dedicated-server/rescue/enable-windows-rdp)

Securing Remote Desktop Protocol (RDP) for System Administrators: [https://security.berkeley.edu/education-awareness/best-practices-how-tos/system-application-security/securing-remote-desktop-rdp](https://security.berkeley.edu/education-awareness/best-practices-how-tos/system-application-security/securing-remote-desktop-rdp)

What’s New in PAN-OS 11: [https://www.paloaltonetworks.com/products/products-a-z](https://www.paloaltonetworks.com/products/products-a-z)

Deploying the Access Server Appliance on VMware ESXi: [https://openvpn.net/vpn-server-resources/deploying-the-access-server-appliance-on-vmware-esxi/](https://openvpn.net/vpn-server-resources/deploying-the-access-server-appliance-on-vmware-esxi/)

Nessus Essentials: [https://www.tenable.com/products/nessus/nessus-essentials](https://www.tenable.com/products/nessus/nessus-essentials)

Download Splunk Enterprise 9.1.1: [https://www.splunk.com/en_us/download/splunk-enterprise.html](https://www.splunk.com/en_us/download/splunk-enterprise.html)

Homelab Idea first inspired by Zach Hill from I.T. Career questions: [https://www.youtube.com/user/PCSimplest](https://www.youtube.com/user/PCSimplest)