---
title: '"Git"ting Started'
keywords: git, github
last_updated: Dec 13, 2018
tags: [getting_started, git, github]
summary: "This documentation is a basic primer in using git.All information here and more can be found in the quintessential source for Git Information: [git-scm.com](https://git-scm.com/book/en/v2).While this source does a far better job at describing and talking someone through any given situation, it is far too extensive and intimidating for many, so this subset is designed to ease you into git with references to this site for more details on key subjects."
sidebar: git_sidebar
permalink: gitstart.html
folder: git
---

Git is a version control software solution.While frequently identified with programming and source code, it is actually able to be used with any resource file - pictures, documents, even executable files themselves (though they typically don't change, so there is little incentive for tracking them in typical use). The intent is to version files - basically track changes from save to save - so that you can revert back to an earlier version at any time.Sometimes you want to revert because of a mistake.Sometimes it is to see glimpses of the thought process.Sometimes it is to revert a wrong path that was taken.The purpose is irrelevant to the software, the key is that there is a chain of versions to go back to for that purpose; and that chain is what Git builds and tracks.

Git does this tracking in a unique way - it uses the concept of snapshots of a file at a specific point in time.These snapshots are then tracked as they change so that at any given point in time, Git knows what version of a file is in use.Instead of going over the details, I recommend you read the [Getting Started - Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics) page to understand how it works and how it is different from other source control solutions (WITH PICTURES!).

Installing Git is very easy.For Linux or Mac, you simply go to the command line and run the appropriate install command:

```bash
$sudo dnf install git-all # (or dnf equivalent) For RPM based systems
$sudo apt install git-all # For Debian based systems

#Mac is slightly different but just as easy from the Terminal:
$git --version
```

Other methods are available as well.

Windows based installations are probably the most difficult ones, but still quite easy.Simply download the installer from the [git-for-windows](https://gitforwindows.org) project on github and run the installer.This will install Git, plus a couple other tools, most useful of which is GitBASH - a linux-like command line utility to interface with Git in the same way as you would in linux environments (more on why this is valuable in a moment).

For those who are afraid of using the command line - I know you are out there! - a good gui tool to frontend Git for Windows is [GitKraken](https://www.gitkraken.com).I don't use it myself, but I have in the past and it works as advertised.The main reason I discourage use of such a tool has nothing to do with the tool, and everything to do with learning what is going on.Basically, by using the command line, you are able to see what is going on at each step and when something goes wrong, you will have the details needed to find information on how to get back on track.Moreover, and the main advantage to having an interface that matches the linux interface, any answers you find online will likely be command line commands - frequently linux (Bash) commands.Using GitBash will allow you to use the same command to do the same thing in the same way.Now, with all that said, once you learn Git well and are comfortable with the command line tooling enough to do some of the more advanced functions, please do look at GitKraken, as some things are just plain easier in a GUI.And it does look nice...Now back to our regular programming...