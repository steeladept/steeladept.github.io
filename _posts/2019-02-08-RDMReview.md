---
title:  "Devolution's Remote Desktop Manager Review"
published: false
permalink: rdm.html
summary: "Remote Desktop Manager from Devolutions is one of my absolute favorite IT tools. Anyone who knows me, knows I talk about how it can help them, and anyone who doesn't know me, quickly finds out if they are doing IT tasks. To save time discussing it, and to help others with the value it provides, here is a breakdown of how I use it and why it is such a huge help."
tags: [RDM, tools]
---

<a href="https://devolutions.net/ref/434933/rdm-home" rel="nofollow" target="_blank"><img src="https://webdevolutions.blob.core.windows.net/images/banners/affiliates/style1-home-300x250.jpg" style="text-align:center;"></a>

Before I get too deep into this, I want to provide a disclaimer - I am a Devolutions Force member and, literally as of today's post, have begun taking part in their affiliate program.  In fact, the link at the bottom of this page is the first affiliate link I have ever provided. Please feel free to follow it to learn more about the many features of RDM (especially the ones I don't cover here).

If it makes a difference, the reason I became a Force member is due to my heavy use of the tool and spreading the word - even before the Force membership and affiliate offers. For those who don't know, Devolutions Force is a free site where members can learn more about the tools Devolutions builds and have fun completing challenges for points which can eventually be redeemed for swag. If you want to join the Force, leave a comment below and I will get in touch. You don't need to go through me, but it is one of the challenges :)

On to the review....  

![alt text:  History Collage][history]

First, a little history - see if this sounds familiar to you. You are an administrator who needs to access many devices (maybe tens, or hundreds, or even thousands of machines).  Each class of device has it's own service and/or perhaps application to interface with it. Many use proprietary protocols, while others use SSH, while still others use RDP. In all cases, you have to open the correct program, and a spreadsheet with the password listed (or preferably some sort of password keeper such as Keepass,LastPass,Secret Server, etc.); then, once you have everything open, you copy and paste the username and password into the appropriate fields (and in some cases you hope you do so fast enough or your clipboard clears!). All this, and you still haven't gained access to do what you need to do - that takes a few more seconds to minutes (depending on device, network latency, password accuracy, etc.) just to find out it didn't work and need to do it again. Was it you? Did you typo something, or accidentally paste in the username in the password field? Or worse, did someone change the password and not update the file. So you try again, and now you are locked out. Okay, now go do the same thing over again, to get the information for the Domain Controller so you can unlock the account, so you can try again. And while you are at it, you change the password so you don't have to do it again later, resetting the 60 day (or 90 day or whatever) timer on the password - you do have your passwords on a rotation, right?

Perhaps, alternatively, you have a master password, and all servers use the same account and password. You know, the same one that has been used for the last 10 years and 15 different administrators have come and gone with it. Nevermind that everyone in the company knows it because it never changes and it helps them get out of that permissions jam, so they use it exclusively now - and yet you constantly have to wonder who made THAT change...

I have been in both situations, and RDM can be instrumental in solving both situations. Let's start with the first situation since that was where I found myself when I first learned about RDM.

When you have a hundred servers in a heterogeneous environment, it can be a challenge to keep straight which ones connect how. RDM solves this by keeping all the information together in a single entry. Connection string, protocol, account information, and even metadata are all maintained in the RDM database as a single entry. In the event you have to string entries together - for VPN for example - one field in the database maintains this connection detail and will reach out to the secondary connection when necessary, and return to the main connection once the pre-connection is complete. Since this is a database, many other pieces of information can be maintained as well, so Devolutions took advantage of that simplicity and provided several optional metadata fields to be used with each connection. For example, since you already know the server you want to connect to, and have the connection information, wouldn't it be convenient to use that same connection list to detail what operating system version is running on that server? Maybe where it is located in the datacenter? Or if it is a physical or virtual server? The information tab provides all that information, and more - lots more (purchase price and server contact information are some of the more obscure optional fields). Best yet, if it is already in your machine information and it is discoverable, RDM can grab that information and populate those fields automagically (okay, some of the magic may require some Powershell scripting, but if you are willing to do that work, it can be done!) Out of the box, it fills in computer name, domain, IP address, and MAC address.

![alt text:  RDM Metadata][metadata]

As if that wasn't enough features, there are a huge number of common tools built in to make troubleshooting the connection that much faster. Need to ping a machine? It's there. Netstat? TraceRoute? They are there. Check running services? Yep. Just look for yourself:

![alt text:  RDMTools][tools]

Now, here is one of my favorite features of RDM. If you are among the more security-minded among us, and cringe at situation #2 above; RDM should start to sound too good to be true, because it really can be your holy grail. What makes RDM stand-out to me, is it really makes things simple for you to follow best practices. Best practices states every server should have a different and difficult password. It should be at least 8 characters long using most of the ASCII character set (the longer the better, and the more varied the better). We all know the shortcuts - using 0 instead of O, ! instead of 1 or l, @ instead of a, etc. Well bad news folks, the bad guys know this too - and they have a lot of hash tables to use those variants to crack a password IF they really are going to do a brute force attack. So when you use a password and intend to protect against brute force attacks, it really does NEED to be something completely random if it is going to help keep bad actors out. RDM has the solution here with their password generator.

![alt text:  Password Generator][generator]

Like most generators, it will produce a variety of passwords using criteria you set. Unlike most generators, it is fully integrated allowing you to save criteria, run the password generation routine, choose a password, and place it in your connection string, all with a click or two. The only thing it doesn't do, is apply that password directly to the server/service it is generated for. In a nutshell, what I am saying is RDM actually makes it easier to follow best practices than to come up with your own password that meets the criteria so you can remember it!

Over time, you find there is far more RDM is capable of; and with the plugin architecture, there is very little it isn't capable of - just some of it you may need to build yourself. But this complexity does come at a cost. Like all software, there are downsides, and for RDM this primarily manifests itself in just how difficult it is to learn. The flexibility RDM offers eases this somewhat, by providing multiple ways to accomplish the same goal, but even that can be a drawback - as there are often reasons to choose one approach over the other. In this event, the flexibility is actually a drawback, as it can provide confusion and/or lead you to build your structure in a way that later needs changed or rebuilt. In the end, however, I have found the benefits FAR outweigh the costs, to the extent that this is one of the very few professional tools I bought for my own personal use. The free version is more than capable for home and light to moderate commercial use, but if you are a systems administrator, developer, or other IT professional where you connect to more than, say 20 or so remote devices regularly; then the Enterprise version is one piece of software to seriously consider adding to your own resource repository. It WILL make you (or your IT staff, if appropriate) more productive.

<a href="https://devolutions.net/ref/434933/rdm-features" rel="nofollow" target="_blank"><img src="https://webdevolutions.blob.core.windows.net/images/banners/affiliates/style2-features-300x250.jpg" style="text-align:center;"></a>

---

[history]: ../images/Banners/History.png "History Collage"
[metadata]: ../images/RDM/RDM_Metadata.png "RDM Metadata"
[tools]: ../images/RDM/RDMTroubleTools.png "RDMTools"
[generator]: ../images/RDM/PasswordGenerator.png "Password Generator"