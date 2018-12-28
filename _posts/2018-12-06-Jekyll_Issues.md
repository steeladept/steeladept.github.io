---
title:  "Jekyll Issues - Issue 1: Linking Pages"
published: true
permalink: jekyllissues1.html
summary: "When originally setting up Jekyll, I ran into many issues.These issues broke my will and I looked around at other solutions.Hugo, in particular, was one I looked at because others I knew, in particular [@arielsanchezmor](https://twitter.com/arielsanchezmor) used Hugo successfully to create his site and keep things rolling. After learning how he worked with Hugo, I started looking at themes for ANY static site generator that fit my needs the way I want it to work.I happened on the Jekyll Document theme and ended up coming back to Jekyll.This blog post is a list of issues I overcame and/or realized after returning to Jekyll and learning the real ins and outs of how I use it."
tags: [news, getting_started, jekyll, troubleshooting]
---

## Problem ##

When I started with Jekyll, one of the things I wanted to be able to do (this is html after all) is cross-link pages for easier reference.External references seemed easy enough, but internal references never seemed to work.I could usually navigate to them if I kept them all on the same level, or even if they were in subfolders that I properly provided paths to, but the links would always fail.

## Resolution ##

The key was that when Jekyll does a build, the resulting site that you are viewing was rendered by the build engine into html files instead of md files. I would try to reference jekyll.md, for example, and it would work, because the browser could read and display .md files.However, the link in the .md file was converted to point to the .html file in the _sites folder; so each time I would reference the link, it wouldn't find a .md file in the _sites folder, and fail.Once I changed the links to .html instead, they mostly worked. FIX - Change the extension the link points to.

I say mostly, because the theme I was using at the time did support permalinks, and while experimenting with them, I had some set.However, in the link, I wouldn't link to the permalink, instead I would try to link to the page title itself.This extension of the original issue took a long time to resolve, and it wasn't until I got deep into the inner workings of the Jekyll Document Theme that I stumbled upon that little error and finally was able to explain why those sites didn't work for me.FIX - don't use permalinks, or reference the permalink instead of the page title.

---