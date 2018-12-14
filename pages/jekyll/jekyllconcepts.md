---
title: Jekyll Concepts And Structures
keywords: jekyll
last_updated: Dec 3, 2018
tags: [getting_started, jekyll]
sidebar: jekyll_sidebar
permalink: jekyllconcepts.html
folder: jekyll
---

## Concepts ##

Jekyll, like all static site generators, is designed to be a simpler way to write and deploy web pages. Jekyll uses the concept of templates to create a recurring format to keep site cohesion and to make updating layout changes across the site quick and easy. This allows the author to focus on the content and creating value on the website without worrying about the design, layout, or a host of other minor issues that are of a repetitive nature not related to the content itself.

To convert content to templates, requires a "build" process, which is nothing more than a small bit of code that reads the templates and the content files and marries them into an HTML page. Template files are written and formatted according to the language the site generator is written in, and the content files are written in a specifically formatted text file - typically markdown.

With Jekyll, specifically, "gems" or pre-created code snippets can be added to extend or modify build functionality.  This is typically used, for example, to distributed and maintain themes, or to provide specific functionality such as Disqus commenting integration. Other solutions usually provide a similar mechanism to extend their functionality as well.

## Structures ##

Jekyll does not generally define a structure per-se, other than those imposed by all static site generators, though there are certain structures prebuilt that should be used if you are going to use that functionality.  One such structure is the blog structure.  However, the biggest structural aspect of Jekyll, as in all static site generators, is the frontmatter.

### Frontmatter ###

Frontmatter is the critical header information added to each content file that tells Jekyll how to use it.  It starts at line one with three dashes (---) and is rarely more than 10 lines or so ending with another three dashes (---).  In between, it typically has a title, layout, and potentially several other variable keywords that are tags for the jekyll build engine to read and respond to.  However none of these variables are required!  Jekyll will go right ahead and respond to the following frontmatter just as easily as one with variables defined:

```text
---
---
```

Unfortunately, it will likely look very bland and not apply any of the features you are likely looking for.  To get the features out of Jekyll you are looking for, and to change certain layouts for different page purposes, you must define them in the frontmatter - either in the content itself, or in the layout that the content is using - so that Jekyll knows how to appropriately apply the settings to the content. For example, here is the frontmatter for this page:

```text
---
title: Jekyll Concepts And Structures
keywords: jekyll
last_updated: Dec 3, 2018
tags: [getting_started, jekyll]
sidebar: jekyll_sidebar
permalink: jekyllconcepts.html
folder: jekyll
---
```

Each variable is defined by the layouts that are used in the theme and each provides a different bit of information that can be used, published, or defined for the build process to modify how the page is created.

See your theme documentation for the purpose and use of each defined variable.

### Content Structures ###

Earlier we mentioned that there are some predefined content structures for blogging.  Jekyll supports blogging using a predefined blog post structure, where the content is named with yyyy-mm-dd-File_name.md and is placed in the _posts folder.  Jekyll recognizes this structure as a blog post and will sort files by the date in newest first fashion.  It also has full structure for how the title is presented, post is dated, and how much of the content is displayed in the teaser.

Another such content structure is pages. These are standard HTML pages not used for blogs or other specialized structures. These are generally free-form structures that other layouts are based off of, and are typically the default layout used when not defined by a different layout.

Finally, there are static files. This is not a structure, per-se, but rather are a class of files that do not contain frontmatter. Things like images, PDFs, or other un-rendered content fall into this category. Knowing what files are static files is important, because you can't apply frontmatter to them directly.  Instead, the only way to apply it is through default properties as setup in the _config.yml file.

### _config.yml file ###

Speaking of which....The _config.yml file is the key file to the entire site.  It is the mayor of the website, defining what can go where, who will use what, and how will it render.  It contains many pieces of information including what theme, if any, to use, what version of Jekyll to build with, where sites will build, what gems can be used, and what default settings are.  This critical file will exist in all Jekyll source files and is read before any Jekyll build starts.  Knowing the _config.yml file is key to starting to know how a site work.