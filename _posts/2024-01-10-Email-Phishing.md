---
title:  "Email Phishing Analysis"
author: Vince Jang
date: 2024-01-10 00:00:00 +0000
categories: [Daily learning]
tags: [Daily learning]
image: /assets/img/picture9.4.png
render_with_liquid: false
---

# Email Phishing
Phishing attack is a type of attack aimed at stealing personal data of the user in general by clicking on malicious links to the users vis email or running malicious files on their computer.  
Phishing attacks correspond to the "Delivery" phase in the Cyber Kill Chain model created to analyze cyber attacks.

## Spoofing
Attackers can send emails on behalf of someone else, as the emails do not necessarily have an authentication mechanism. Attackers can send mail on behalf of someone else using the technique called sppofing to make the user believe that the incoming email is reliable. Serveral protocols have been created to prevent the Email spoofing technique.  
With the help of SPF, DKIM and DMARC protocols, it can be understood whether the sender's address is fake or real. Some mail applications do these checks automatically. However, the use of these protocols is not mandatory and in some cases can cause problems.  
To find out manually whether the mail is spoof or not, SMTP address of the mail should be learned first.  
Thanks to Mxtoolbox to enable to comparing the MX records, whether the mail is spoof or not.

## Email Header
"Header" is basically a section of the mail that contains information such as sender, recipient and date. also there are fields such as "Return-paht", "Reply-To" and "Received".  
It looks like this.  
![images](/assets/img/picture9.PNG)  

## Breakdown Header
as above Image we can see many different field in header  
1. From, To field is determined from whom an email will go to whom
2. Date field is the timestamp that shows when the email was sent.
3. Subject is mentions the topic of the email.
4. Return-Path is the field that if you reply to an email, it will go to the address mentioned in the Return-path.
5. Domain Key and DKIM signatures are email signatures that help email service providers identify and authenticate your emails, similar to SPF signatures.
6. Message-ID is a unique combination of letters and numbers that identifies each mail
6. MIME-Version is an internet standard of encoding. it converts non-text content like images, videos and other attachments into text so they can be attached to an email and sent through SMTP.

## Email Header Analysis

I learned There are 2 key questions we need to answer when checking headings during a phishing analysis
1. Was the email sent from the correct SMTP server?
2. Are the data "From" and "Return-path / Reply-to" the same?  

### Was the email sent from the correct SMTP server?
![images](/assets/img/picture9.1.PNG)  
We can check the "Received" field to see the path followed by the mail. as the immage shows the mail is "222.227.81.181" from the IP address server.  
If we look at who is sending the mail(sender), we see that it came from the domain "mx.zaq.ne.jp"  
So "mx.zaq.ne.jp" should use, "222.227.81.181" to send mail. To confirm this, we can query the MX server actively used by "mx.zaq.ne.jp"  
"mxtoolbox.com" helps by showing you the MX servers used by the domain you searched.  
![images](/assets/img/picture9.2.PNG)  

if we look at the image above, the "mx.zaq.ne.jp" domain uses different server. so there is no relationship with the  "mx.zaq.ne.jp" or "222.227.81.181" address.  
  
In this check, it was determined that the email didn't come from the original address, but was spoofed.  

### Are the data "From" and "Return-path / Reply-to" the same? 
In most of cases, we expect the sender of the e-mail and the person receiving the responses to be the same  
Someone sends an email(gmail, hotmail etc) with the same last name of someone working for google to Jcom(zaq.ne.jp), Jcom tells the employee that he has issued the invoice and they must make the payment to his XXX account. it puts the email address of the real Google employee in the "Reply to" field so that the fake email address does not stand out in case of replying to a possible email.  
So! we have to do compare the email addresses in the "From" and "Reply to" field.  

![images](/assets/img/picture9.3.PNG)  
as you can see, the data is different. when we want to reply to this e-mail, we will send a reply to the "daum" address below. just because this data is different doesn't always mean it's definitely a phishing email, we need to consider the event as a whole. in addtion to this suspicious situation, if there is a harmful attachment, URL or misleading content in the e-mail content, we can understand that the e-mail is phishing.

