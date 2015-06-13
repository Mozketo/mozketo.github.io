---
layout: post
title: Migrating from Wordpress to Github Pages
date: 2015-05-24 15:41
author: bclarkrobinson
comments: true
categories: github
---
This blog was recently running on a self-hosted Wordpress site and found myself dispirited by the user experience of Wordpress. For all its flexibilty and functionality I would find myself shying away from blogging for the following reasons:

* The tools for writing and editing posts - although always improving - compare poorly to my favourite text editor(s),
* Maintaining security updates and plugins is such a constant frustration,
* All those posts aren't in any form of source control but instead a MySQL database that needs a *backup strategy*,
* Wordpress was missing the *fun* factor.

<!--more-->

After looking around at several options I decided on using Github Pages with Jekyll. This option was going to help on all of these points. I can use my favourite text editor(s) with Markdown, no more reminders for security updates, git replaces the DB, and the fun factor is back. Yay :)

Here's my notes to getting started with Jekyll and Github pages.

---

### Setting up Jekyll on OS X Yosemite

First up let's setup OS X Yosemite so that we can get Jekyll running. To do this we'll need to install a few components so that we can compile the dependancies. Open a terminal window and execute:

xcode-select -install

_Note: I'm using the verion of Ruby that ships with Yosemite and won't cover using Homebrew or RVM etc._

Install Jekyll by using a terminal window again to execute the command:

sudo gem install jekyll

[Recommended reading](https://help.github.com/articles/using-jekyll-with-pages/)

---

### Setup a Github Page

1. Follow the guide from [Github](https://pages.github.com/), it's really easy to follow along.
2. Don't forget to add a [.gitignore](https://github.com/Mozketo/mozketo.github.io/blob/master/.gitignore) like this otherwise you'll accidently commit the _site directory.

---

### Pick a theme

1. Search around for Jekyll themes. For this blog I settled on <https://github.com/muan/scribble>,
2. Fork or add it to your Github page repo.

---

### Start posting

1. Create a _posts directory in the repo,
2. Adding a file like _YYYY-MM-DD-this-is-my-post-title.md_ into _posts,
3. Ensure that the Front Matter header is at the top of the post <http://jekyllrb.com/docs/frontmatter/>,
4. For more info read along with the Jekyll [docs](http://jekyllrb.com/docs/posts/).

---

### Setting the DNS for GitHub Pages on Namecheap

To complete the migration I followed these articles to configure github with a custom URL:

1. <http://davidensinger.com/2013/03/setting-the-dns-for-github-pages-on-namecheap/> and 
2. <https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/>

Good luck and have fun. :)