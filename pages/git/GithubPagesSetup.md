---
title: GitHub Pages Setup
keywords: GitHub Pages, jekyll, github, markdown
last_updated: Dec 12, 2018
tags: [getting_started, jekyll, github, markdown, github_pages]
summary: "Learning how to setup and start a blog hosted on GitHub Pages"
sidebar: git_sidebar
permalink: GithubPagesSetup.html
folder: git
---

Setup should be easy. The documentation is easy, the site is basic, there really isn't a lot to it.If you can create and manage SSH Keys, that is something like 50% of the work.

For me, however, it wasn't so simple. GitHub Pages is built on the Jekyll static site generator. That means you have a whole host of themes and site designs at your disposal and all you need to do is add content. Unfortunately, it does take away some of the flexibility of building your own site, and there is all the dynamic content possibilities that no longer are an option. The reality is if you want to do anything that deviates from the base templates in the theme you choose, you need to learn a whole lot of Jekyll to get it working the way you want. And if you don't like your theme options, there are tons of alternatives, but that is a whole other layer of learning.

But before we get too far ahead of ourselves, lets discuss the details to get the basic site up and running.We will discuss the other pieces in the [Jekyll](/jekyll.html) tutorial.

>Step 1 - Get a GitHub account.Just make your way to [github.com](https://gitub.com), create a username, provide your email, and create a password.When prompted, chose your account type.For these purposes, we are using the "Free" option.Finish the next pages or skip to step 2.

>Step 2 - Start a project.Once your account is created, press the Start a project button.the next page will prompt you for a repository name and description.Also, you can keep it public or make it private (for our purposes, we will need to keep it Public as the Free option does not allow for private GitHub Pages.)  NOTE:  It is likely beneficial to add the .gitingore to your repo.This isn't strictly necessary, but I always found it handy, and when I accidentally miss this step, I end up creating one later anyway.

>Step 3 - Once the Repo is created, you can connect to it via your preferred method.GitHub provides several methods, but I usually just use GitBash on windows or the git tool from the Linux distro I am using.

Yea!  GitHub is now setup and ready for pushing or pulling source controlled information!

This is awesome for developers, or others just looking for source control, but we are here to setup GitHub Pages to act as our website.So that means our work is only partly done.Now that we know we can publish documents to GitHub, we now need to setup our GitHub Repo to be a GitHub Pages provider.To do this, we go to the *Settings* page as shown.

![Settings](/images/GitHubSettings.png)

From this page, scroll all the way to the bottom.Once there, you will see a box of red which has a lot of options for deleting the repository and other equally destructive options. The box just above this "Danger Zone" is the GitHub Pages box.

In the GitHub Pages box, all you need to do to enable the pages is change the *None* setting to either *master branch* or *master branch /docs folder*.Since this will hold our site, and not all our source files, just choose *master branch*.

---

NOTE:  There may be a way to use the /docs folder for the site and store the pages in the same space overall, however I have not attempted this setup and do not know how successful you may or may not be doing so.

---

The last thing you need to do is choose a theme and publish your site.GitHub Pages only supports a handful of different themes from their settings.If one of these fits your design, you are in luck, and can just use what is provided.If you want a different one, however, you need to learn a bit more about Jekyll and how to use external themes with GitHub Pages.

If that last line describes you, I got you covered in my [Jekyll](/jekyll.html) pages.