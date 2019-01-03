---
title:  "IT Tooling - Part 1"
published: true
permalink: ittools1.html
summary: "Like any profession, IT has its fair share of tools. Below is a list of my favorites in various categories and why."
tags: [news, tools]
---

Choosing favorite tools is always a very personal and task specific operation.What works great for me, is just so so for someone else.Still, it can often prove useful to learn of a few new tools, or at least to see what tools professionals use and how, to perhaps up your game.When I choose a tool, I look first and foremost at it's efficiency of MY time.If it takes 20 minutes to do a task, but only 3 seconds for me to setup, that is far better than one that only takes 15 minutes to complete, but 10 of it was me setting it up.Another often used aspect of measuring efficiency is how much do I have to remember to use the software?  If it takes me an hour to setup each time I need to make a change, but only one click each time I use it once it is setup, this is far more efficient use of my time than 5 minutes every time I need to do the task.

Once I know how efficiently my time can be utilized with this software, I look at how broadly usable it is.In this part, I look at cross platform vs. single platform tools, how many usable functions it provides (and how often I use those functions), and general usability.Finally, I look at cost - because it may be awesome, but I don't have $2500 to drop on Adobe Tools when I can get GIMP for free and learn my way around it...eventually. (Yes, I know, I exaggerated the example. You get the idea I am conveying).

I have broken out the tools by category below.There are many different tools for many different purposes, look for part 2 as a continuation of the various categories of tools.

## Favorite Desktop Operating System ##

It's a lie - I hate them all equally for vary different reasons. Okay, not equally. Mac I haven't dealt with much, so I hate dealing with it more than the others because I never can do what I want with it due primarily to lack of familiarity. Perhaps one day I will change my mind, particularly if Apple ever lowers the "MacTax" premium for their hardware and software. But until then, I like it far less than any Linux or Windows system.

Linux is great, until it isn't.There is so much, but the quality of the software is everywhere.And discovering new items, tricks, tips, etc... Good luck.There is either too many or not enough places to look, depending on your approach.More concisely, too many obscure places to look, and not enough mainstream places to look for new information. Saying Linux, generically, however, is a bit like saying I like living in a house - it doesn't tell you much about it.Linux has so many distributions, that to be useful, you need to know the distribution to make a statement about what you look for in Linux.To this end, I throw my preference toward Mint Linux Debian Edition.I just find it easier to use and easier to work with than any other desktop Linux.A close runner up is Fedora, but that is due to my familiarity with CENTOS and the RedHat family of servers in general.

Windows is like Linux in that it is great, until it isn't.The biggest gripe I have here is the monolithic nature.It pulls together far too many pieces into a single, tightly coupled system that doesn't always work as expected, but can't be easily fixed either.To be fair, it is getting better on this point, but that is as it becomes more Linux-like and progressively less discoverable.

Favorite:  Tie between [Mint Linux LMDE](https://www.linuxmint.com/download_lmde.php) and [Windows 10](https://www.microsoft.com/en-us/windows/get-windows-10?step=OSresultsBusiness) depending on the day and the task.

## Favorite Password Manager ##

I put this as my second category because once you have the platform installed (the OS listed above), then next most important tool to me is my password manager. There is a variety of reasons for this - So I can add passwords as I create them, because keeping password best practices is important, and because it is easier to setup my connections, password, and all other components at the same time. My favorite password manager, Remote Desktop Manager from Devolutions, makes this even easier still by combining each of these features into a single connection management tool that allows me to create connections between any two systems (or nearly so), manage the password, and even how I connect. For example, on my work laptop I can create a connection to AWS using the built in Jump Server connection to run it through the only open connection to the AWS endpoint.If I can not connect to the Jump Server, say because I am working from home, I can even configure the system to test the connection, and if it isn't there, connect via VPN first, then connect to the Jump Server, and tunnel through to the AWS server where I need to work.All this requires a little setup, which is required regardless of how you do this, but instead of remembering all those steps each time and making three different connections with three different accounts and password, I set it up once, and when I need to connect, I just double-click on the connection, without worry as to how to make that connection again. Moreover, the connection can be reused, if setup to do so. In the example, the VPN could be a secondary connection that is used sometimes, but can be used for ANY connection back to the office, not just to the jump server.

Making each connection simple is just one feature of this software that I love. It has a built in password manager, based on your choice of databases, or can connect to a wide variety of external databases, including competing products. I just started a new contract with another company but they use Keepass instead of LastPass - how annoying right?  Not with RDM.You just point your connections to Keepass instead of LastPass, make the integration connection, and use it the exact same way. Another company - a managed service provider - has a separate password database for each customer they manage.Not a problem, RDM supports multiple database connections, and if you need this functionality, but don't have a solution in place, Devolutions offers the Devolutions Server which will help manage those databases and connections without your end users ever being able to see what the actual password is.In fact, if it is just a password manager you need, the newly built in Launcher prevents the need for a separate client.However this is getting away from RDM.

RDM is really a full connection management tool to manage connections over a variety of protocols in a variety of configurations, complete with account information, and secure solutions to meet a variety of requirements.It is because of this broad and deep capability built into a single tool to make an administrators life easier that I have chosen this as my favorite password manager.Unfortunately, it does not run on Linux, even in WINE for the most recent versions.I look forward to .Net Core growing up and RDM being refactored to work cross platform with any Linux operating system.For Linux users, the project I have been recommended to use is [Remmina](https://remmina.org), which is similar, but far less comprehensive.In my experience, the repositories break a lot as well, so there may be some work involved with using this project.

Favorite:  [Devolutions Remote Desktop Manager](https://remotedesktopmanager.com)

As you can imagine, this subject can get quite lengthy, quite quick.This ends part 1 of this series of posts.Look for [part 2](./ittools2.html) coming soon where we will review several more categories of tools. In the mean time, if you have a favorite OS or favorite Password Manager, leave a comment below about what you like about it and educate us on the merits of your tools of choice.

---