---
layout: post
title: sudo in OS X with non-admin account
date: 2008-11-25 11:45
author: bclarkrobinson
comments: true
categories: [Apple]
---
I try to keep my Mac nice and secure so I run my everyday account <em>ben</em> with non-administrator privileges and keep an <em>admin</em> account so when I install Apps or use Software Update I will be asked for the admin user and password. This works just fine, but when I attempt to
<pre>sudo</pre>
from the command-line: Fail.

For example I was attempting to update Rails to version 2.2;
<pre>gem update rails</pre>
this of course failed, so I tried;
<pre>sudo gem update rails</pre>
When asked for my password, the <em>admin</em> password would fail, and my <em>ben</em> password would fail too with the error "ben is not in the sudoers file". I expected the <em>ben</em> everyday-user to fail, but how to get my <em>admin</em> user to step up to the plate and take over?

Well I found the secret from <a href="#mce_temp_url#">here</a> and it worked beautifully. Simply type
<pre>su &lt;admin username&gt;
sudo &lt;command&gt;
password: &lt;admin password&gt;</pre>
Yipee, sudo from OS X terminal, finally.

And type 
<pre>exit</pre>
to restore to your previous account prior to running su.
