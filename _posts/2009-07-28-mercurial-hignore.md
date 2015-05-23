---
layout: post
title: Mercurial .hgignore for C# and VB .net Projects
date: 2009-07-28 21:26
author: bclarkrobinson
comments: true
categories: [Mercurial, Source Control]
---
There's certain files and folders that you don't want to put into source control. With <a href="http://mercurial.selenic.com/wiki/">Mercurial </a>all you need is a .hgignore file in the root path of your project. A simple windowsy way of doing this is (from a command line) is by typing

<code>notepad c:\path_to_code\.hgignore</code>

This will create the file so now simply add the following lines to the file:
<pre lang="DOS" colla="+">
# use glob syntax.
syntax: glob

# Misc Mac/Windows stuff

.DS_Store
Thumbs.db
Desktop.ini

# builds

*.exe
*.ex_

# vb6

*.SCC
*.vbw
*.pdb
*.log
*.Log

# c-sharp
#ProjectName/bin
#ProjectName/obj
*.user
*.suo
_ReSharper.*
*.sln.cache
</pre>

<strong>Note:</strong> You need to change #ProjectName to your project foldername.

Now feel free to init/add/commit:

<pre lang="DOS" colla="+">
hg init
hg add
hg commit -m "Initial Commit with ignores"
</pre>
