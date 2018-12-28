---
title:  "Jekyll Issues - Issue 2: Special Characters"
published: true
permalink: jekyllissues2.html
summary: "When originally setting up Jekyll, I ran into many issues.These issues broke my will and I looked around at other solutions.Hugo, in particular, was one I looked at because others I knew, in particular [@arielsanchezmor](https://twitter.com/arielsanchezmor) used Hugo successfully to create his site and keep things rolling. After learning how he worked with Hugo, I started looking at themes for ANY static site generator that fit my needs the way I want it to work.I happened on the Jekyll Document theme and ended up coming back to Jekyll.This blog post is a list of issues I overcame and/or realized after returning to Jekyll and learning the real ins and outs of how I use it."
tags: [news, getting_started, jekyll, troubleshooting]
---

## Problem ##

As I was posting these pages internally, I kept having issues with one of my Git posts.It is titled "Git"ting started, and the quotes are included.Since quotes aren't generally an issue, I didn't think anything of it, but when I would build Jekyll, it kept placing the page in a very different place, causing the page to come up as **404 - File not found** whenever I would build the site and navigate to where that page should be.I had no other issues with any pages, and I verified it was setup exactly the same.

I kept getting confused, so I looked at the site, and sure enough, the html file was not where it should be.Instead, it was 3 folders deep in a _site/pages/git/getstart.html. My next stop was the command line where Jekyll built the site.I was getting a **'YAML Exception .../pages/git/gitstart.md: `(<unknown>)`: did not find expected ky while parsing a block mapping at line 2 column 1'**.What the heck does that mean?

## Resolving Issue 2 ##

Well at least the error gave an address.Looking at line 2, I saw it was the title line in the frontmatter of the file.That title had the quotes in it.I thought to myself, "I wonder if that is what is causing the issue".My scripting experience has seen tons of issues with characters such as these, and in many cases you need to escape them - however they are not typically an issue with .md files.However, with Jekyll....

But no, that can't be it.After all, I have quotes in my summary, quotes around other titles, there are quotes everywhere in my frontmatter.That can't be it, or can it?  I mean the quote does end in the middle.And it isn't like the site is coming up at all, so maybe the string is confusing it.Getting ready for a long trip of StackOverflow reading, I tried the easy fix first:

So I enclosed the whole title in single quotes -'"Git"ting Started'

Recompiled Jekyll, and no more issue.What's more, it shows on the page, just as I would expect.No single quotes, with the double quotes. --*Win!*--

Now I have more time for the next blog.

---