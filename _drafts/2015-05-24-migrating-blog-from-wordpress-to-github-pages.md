---
layout: post
title: Migrating my blog from Wordpress to Github Pages
date: 2015-05-24 15:41
author: bclarkrobinson
comments: true
categories: github
---
# Migrating data from Wordpress to Jekyll

This blog was recently running on self-hosted Wordpress site and would find myself shying away from blogging because:

* The tools for writing and editing posts inside wordpress - although always improving - wasn't my favourite text editor,
* Maintaining security updates and plugins is such a constant frustration,
* All those posts aren't in any form of source control but instead a magic MySQL database somewhere in magic land,
* Was missing *fun* factor.

Setting up Github Pages & Jekyll was going to help on all of these points. I can use my favourite text editor(s) with Markdown, no reminders for security updates as pages simply render as HTML, git replaces the DB, and the fun factor is back.

# Setting up Jekyll on OS X Yosemite

First up let's setup the OS X environment so that we can get Jekyll running. To do this we'll need to install a few OS components so that we can compile the dependancies. Open a terminal window and execute:

xcode-select -install

_Note: I'm using the verion of Ruby that ships with Yosemite and won't cover using Homebrew or RVM etc._

Now to install Jekyll:

sudo gem install jekyll

[Recommended reading](https://help.github.com/articles/using-jekyll-with-pages/)

# Setting the DNS for GitHub Pages on Namecheap

To complete the migration I followed these articles to correctly configure github with a custom URL. [http://davidensinger.com/2013/03/setting-the-dns-for-github-pages-on-namecheap/] and [https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/].