---
title:  "Write up - Shellshock Attack [Letsdefend]"
author: Vince Jang
date: 2024-04-29 00:00:00 +0000
categories: [writeup]
tags: [writeup]
image: /assets/img/pic14.6.PNG
render_with_liquid: false
---

## Shellshock Attack

### Note
1. Log file: /root/Desktop/ChallengeFile/shellshock.pcap
2. Wireshark  

## What is shell shock attack?
Shellshock attack is the vulnerability in Bash allows remote code excution without confirmation. A series of random character () { :; }, confuses Bash because it doesn't know what to do with them, so by default, it excutes the code after it.  
Window is completely safe from this vulnerability. about 75% of the internet is Apache, and 80% of Apache server runs on Linux, so almost the entire Internet is vulnerable.  
Bash functions can be used in .sh scripts, one liner commands and can also be defined in environment variables.  

### 1. What is the server operating system?
Okay, let's have a look pcap file.  
![image](/assets/img/pic14.2.PNG)  
I can see an HTTP packet that includes 500 error message. this usually contains some information such as OS system, version and server information.  
So I went to check this packet and Boom!  
![image](/assets/img/pic14.3.PNG)  
Found the Gold data!  

### What is the application server and version running on the target system?
I didn't even have to check other packets because I can already see the answer in this packet!  
![image](/assets/img/pic14.3.PNG)  

### What is the exact command that the attacker wants to run on the target server?
For this we need to check what made this server to request the 500 Error message. (Attacker IP is 10.246.50.2)  
![image](/assets/img/pic14.4.PNG)  
Let's check the other http packet.  
![image](/assets/img/pic14.5.PNG)  
The attacker was attempting to access the file on the server named /exploitable.cgi. I observed that the user-agent request included random characters and a command. 
  
Thank you!


