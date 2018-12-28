---
title: Beginning the Journey
keywords: jekyll
last_updated: Dec 3, 2018
tags: [getting_started, jekyll]
sidebar: jekyll_sidebar
permalink: jekyllstart.html
folder: jekyll
---

## How this started.... ##

Like most things related to this blog, I started attending the [Pittsburgh LittleHacks](https://capozza.io/little-hack/) where I learned of using Markdown with GitHub Pages to host a blog site which seems to be the right combination for ease of use, cross platform, broad availability, and portability for my needs. GitHub pages is an interesting and useful utility to allow anyone to start building up a static website free of charge (GitLab has a similar feature if you prefer that over GitHub). There are some restrictions to it, of course, but largely they are easy to deal with for the casual user or individual just wanting a place to put their writing.If you need more, checkout [Blogger](https://www.blogger.com) or a host of other options to place your musings.For me, and many like me, this is enough, appropriate, and best of all free.

## Setup ##

Setup should be easy. The documentation is easy, the site is basic, there really isn't a lot to it.If you can create and manage SSH Keys, that is something like 50% of the work.

Unfortunately, for me, it wasn't so simple. The thing is, GitHub uses Jekyll as the basis of their GitHub Pages.If you stick with a simple page or two, using one of the 4 themes they offer, and are not looking for anything more complicated than that, well you are in luck - it is easy.However, if you want to use a different theme, or want to create your own layouts, or do anything more complicated than write an MD file and have it show up in a browser, then there is a lot more work.

In my very first LittleHacks meeting, I was shown how to set this up and use it with an alternative to Jekyll, called Hugo.Trying to simplify this approach, I stuck with Jekyll and tried to bash it into the way I wanted it to work, which resulted in failure.It simply does not support direct upload of MD files and build out your sites as I had hoped it would do.Instead, you have to maintain a structure, place files in the right place, use the right naming convention, and most of all, be sure you have the right frontmatter setup!

Instead, Jekyll broke me trying to make it work the way I wanted it to.So I started looking around - Hugo, Hexo, and even Middleman before I came back to Jekyll.The key, for me, was finding a good theme that I could follow, that I understood what it was doing and why, and could edit to fit my needs if necessary.The [Jekyll Document Theme](https://github.com/tomjoht/documentation-theme-jekyll) fit that need, with unprecedented documentation on what was going on to help me learn Jekyll while making the theme fit my purpose.