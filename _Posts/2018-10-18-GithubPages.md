---
title: "GitHub Pages"
date: 2018-10-18
categories: [Markdown, GitHub, Jekyll]
---

# Building and Maintaining GitHub Pages #

GitHub pages is an interesting and useful utility to allow anyone to start buiding up a static website free of charge.  There are some restrictions to it, of course, but largely they are easy to deal with for the casual user or individual just wanting a place to put their writing.  If you need more, checkout [Blogger](https://www.blogger.com) or a host of other options to place your musings.  For me, and many like me, this is enough, appropriate, and best of all easy.

## Setup ##

Setup should be easy.  The documentation is easy, the site is basic, there really isn't a lot to it.  If you can create and manage SSH Keys, that is something like 50% of the work.

Unfortunately, for me, it wasn't so simple.  (Will complete later)

## Maintaining the pages ##

So after all that was done, there is this simple little issue of using it.  I started using Markdown when I started the project, and found it quite useful and easy to pickup.  I was at a job where I had to use GIT a lot (not GitHub, but the actual software that runs the version control of the site), and so I started getting used to that too.  So once I got all the details worked out on the setup it was easy, right?  Well....

Honestly, the issues I have had are very minor and easy to resolve so far.  They are known restrictions with GitHub Pages as detailed in the documentation and now I know it was because I just wasn't following them.  But that doesn't mean it didn't drive me nutty working at it before I figured out I was the stupid one here (well I knew it before hand, I just didn't know how I was being stupid).  Perhaps in your wisdom, dear reader, you will learn a little nugget here so you are not going as crazy as I did.

My first issue was linking.  More specifically, my home page (the README.md if you didn't already know by now) was being used as an index of indexes.  Using Markdown linking, I was able to make it work in VSCode just fine, and so I knew it was correctly formatted, yet when I pushed the updates up to the website...poof!... no pages.

Grrr... Time to go to Github and see what didn't make it.  Wait, it is there. Oh, yeah.  \<Refreash web page>  There it...isn't?  What?  I mean it is there, but it isn't being prettified.  It is pure text and that is it.  Like the CSS isn't being applied.  I don't get it.  It worked in VSCode, it worked with the Markdown Syntax, it worked with everything, why isn't it working in GitHub?

My first thought is "Oh yeah, isn't one of those restrictions I spoke of earlier, one where you can't have subfolders?"  So I qucikly moved all files to the same level.  It is messy, and I need to rename the README.md files so they don't overwrite each other, but that was easy enough.  Now it should work!

And in comes my second issue - or is it an extention of the first.  Either way, it still doesn't work!  The home page works fine, but any secondary page, nope! A quick google search showed me that it isn't going to be THAT easy.  I actually have to LEARN Jekyll...




[Things that make you go Hmm....](https://binged.it/2Ae4ht6)