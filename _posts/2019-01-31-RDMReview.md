---
title:  "Devolution's Remote Desktop Manager Review"
published: false
permalink: rdm.html
summary: "Remote Desktop Manager from Devolutions is one of my absolute favorite IT tools. Anyone who knows me, knows I talk about how it can help them, and anyone who doesn't know me, quickly finds out if they are doing IT tasks. To save time discussing it, and to help others with the value it provides, here is a breakdown of how I use it and why it is such a huge help."
tags: []
---

![alt text:  RDM Banner][rdmbanner]

Before I get too deep into this, I want to provide a disclaimer - I am a Devolutions Force member and, literally as of today's post, have begun taking part in their affiliate program.  In fact, the link at the bottom of this page is the first affiliate link I have ever provided. If it makes a difference, however, the reason I became a Force member is due to my heavy use of the tool and spreading the word - even before the Force membership offer.  (For those who don't know, Devolutions Force is a free site where members can learn more about the tools Devolutions builds and have fun completing challenges for points which can eventually be redeemed for swag. If you want to join the Force, leave a comment below and I will get in touch. You don't need to go through me, but it is one of the challenges :)

On to the review....  

![alt text:  History Collage][history]

First, a little history - see if this sounds familiar to you. You are an administrator who needs to access many devices (maybe tens, or hundreds, or even thousands of machines).  Each class of device has it's own service and/or perhaps application to interface with it. Many use proprietary protocols, while others use SSH, while still others use RDP. In all cases, you have to open the correct program, and a spreadsheet with the password listed (or preferably some sort of password keeper such as Keepass,LastPass,Secret Server, etc.); then, once you have everything open, you copy and paste the username and password into the appropriate fields (and in some cases you hope you do so fast enough or your clipboard clears!). All this, and you still haven't gained access to do what you need to do - that takes a few more seconds to minutes (depending on device, network latency, password accuracy, etc.) just to find out it didn't work and need to do it again. Was it you? Did you typo something, or accidentally paste in the username in the password field? Or worse, did someone change the password and not update the file. So you try again, and now you are locked out. Okay, now go do the same thing over again, to get the information for the Domain Controller so you can unlock the account, so you can try again. And while you are at it, you change the password so you don't have to do it again later, resetting the 60 day (or 90 day or whatever) timer on the password - you do have your passwords on a rotation, right?

Perhaps, alternatively, you have a master password, and all servers use the same account and password. You know, the same one that has been used for the last 10 years and 15 different administrators have come and gone with it. Nevermind that everyone in the company knows it because it never changes and it helps them get out of that permissions jam, so they use it exclusively now - and yet you constantly have to wonder who made THAT change...

I have been in both situations, and RDM can be instrumental in solving both situations. Let's start with the first situation since that was where I found myself when I first learned about RDM.

---

[rdmbanner]:  images/Banners/ "RDM Banner"
[history]: ../images/MiscImages/ "History Collage"