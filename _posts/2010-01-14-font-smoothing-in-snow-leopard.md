---
layout: post
title: Font Smoothing in Snow Leopard with a 3rd Party LCD
date: 2010-01-14 10:11
author: bclarkrobinson
comments: true
categories: [Apple]
---
Having connected a 3rd party (Polyview) 19" LCD into the work MacBook Pro I was struck by how poor any on screen text rendered. Trying to use Terminal.app was just aweful, all jaggy and hard to read.

It turns out that font smoothing (aka sub-pixel antialiasing) on the 3rd party screen wasn't happening and OS X was defaulting the font smoothing to CRT. Take a look at the screenshots below, bad eh?

[caption id="attachment_283" align="alignnone" width="638" caption="Antialiasing for CRTs, ewwwww."]<a href="http://mozketo.com/wp-content/uploads/2010/01/Screen-shot-2010-01-14-at-9.41.38-AM.png"><img src="http://mozketo.com/wp-content/uploads/2010/01/Screen-shot-2010-01-14-at-9.41.38-AM.png" alt="" title="Screen shot 2010-01-14 at 9.41.38 AM" width="638" height="61" class="size-full wp-image-283" /></a>[/caption]

[caption id="attachment_284" align="alignnone" width="637" caption="Antialiasing fixed!"]<a href="http://mozketo.com/wp-content/uploads/2010/01/Screen-shot-2010-01-14-at-9.42.53-AM.png"><img src="http://mozketo.com/wp-content/uploads/2010/01/Screen-shot-2010-01-14-at-9.42.53-AM.png" alt="" title="Screen shot 2010-01-14 at 9.42.53 AM" width="637" height="56" class="size-full wp-image-284" /></a>[/caption]

<h3>How to fix the problem?</h3>

Open up terminal and paste in the following:

<code>defaults -currentHost write -globalDomain AppleFontSmoothing -int 2</code>

or if you find the text a little large/blurry you can try:

<code>defaults -currentHost write -globalDomain AppleFontSmoothing -int 1</code>

Now logout > login.

<h3>So what's up?</h3>
 
The whole Snow Leopard font smoothing issue seems strange and why Apple changed the font smooth dialog is beyond me. But you can read more about it here 
<a href="http://blog.jjgod.org/2009/08/18/snow-leopard-vs-dell-lcd-displays/">jjgod / blog > Snow Leopard vs. 3rd Party LCD Displays</a> if you'd like to know a little more about the problem.

