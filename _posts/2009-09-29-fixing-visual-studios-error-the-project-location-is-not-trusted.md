---
layout: post
title: Visual Studio's error 'The project location is not trusted'
date: 2009-09-29 22:27
author: bclarkrobinson
comments: true
categories: [Uncategorized]
---
Scenario: My base machine (mac) hosts all my source code as I want a single code point to backup. My Windows development environment is in a Virtual Machine using VMWare Fusion. (I believe this issue will apply to VMWare Workstation, Player and Fusion). 

I am using the VMWare built in "Share Folder" option, and I'm sharing only the "win" folder in my ~\src\ folder.

<a href="http://mozketo.com/wp-content/uploads/2009/09/Screen-shot-2009-09-29-at-10.19.55-PM.png"><img src="http://mozketo.com/wp-content/uploads/2009/09/Screen-shot-2009-09-29-at-10.19.55-PM.png" alt="Screen shot 2009-09-29 at 10.19.55 PM" title="Screen shot 2009-09-29 at 10.19.55 PM" width="496" height="92" class="alignnone size-full wp-image-246" /></a>

Issue: When trying to open source using Visual Studio 2008 I'm presented with the dreaded "The project location is not trusted" error.

Solution: Open a Command-Line box (Start > Run > cmd > OK)

<pre lang="DOS" colla="+">
cd \Windows\Microsoft.NET\Framework\v2.0.50727
caspol -m -ag 1.2 -url \\.host\* FullTrust
</pre>

If you're not using VMWare or have a share elsewhere substitute "\\.host\*" for your shared folder.

References: <a href="http://msdn.microsoft.com/en-us/library/bs2bkwxc.aspx">The project location is not trusted dialog box</a> and <a href="http://social.msdn.microsoft.com/Forums/en-US/vssetup/thread/5065fd7c-f2ed-4ddc-8242-19c0eda2a1a1">The project location is not trusted dialog box when trying to use a Network Location</a>
