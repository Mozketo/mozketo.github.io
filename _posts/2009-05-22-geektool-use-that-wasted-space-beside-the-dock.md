---
layout: post
title: Geektool - Use that wasted space beside the Dock
date: 2009-05-22 16:01
author: bclarkrobinson
comments: true
categories: [Apple]
---
One little gripe I've always had about in OS X is the apparent waste of space either side of the Dock (assuming you have it centred at the bottom of the screen).

<a href="http://projects.tynsoe.org/en/geektool/">Geektool</a> to the rescue!

<a href='http://www.quicksnapper.com/Mozketo/image/-dock-w-weather-and-stats'><img src='http://www.quicksnapper.com/files/4932/14717678234A163D4C55EA8_m.png' alt='View the image at QuickSnapper.com' title='Hosted by QuickSnapper.com' width="505" /></a>

Here's the little scripts that I use to get the weather (just change the region code ASXX0016 to your region from <a href="http://weather.yahoo.com">weather.yahoo.com</a>. Your region code will be found in the yahoo URL):

<pre lang="Bash" colla="+">
curl --silent "http://xml.weather.yahoo.com/forecastrss?p=ASXX0016&u=c" | grep -E '(Current Conditions:|C<BR)' | sed -e 's/Current Conditions://' -e 's/<br \/>//' -e 's/<b>//' -e 's/<\/b>//' -e 's/<BR \/>//' -e 's/<description>//' -e 's/<\/description>//'
</pre>

<pre lang="Bash" colla="+">
curl --silent "http://xml.weather.yahoo.com/forecastrss?p=ASXX0016&u=c" | grep -E '(High:)' | sed -e 's/<BR \/>//' -e 's/<b>//' -e 's/<\/b>//' -e 's/<BR \/>//' -e 's/<br \/>//'
</pre>

And the script for Memory usage, HDD space and Uptime:

<pre lang="Bash" colla="+">
top -l 1 | awk '/PhysMem/ {print $10}' 
df -hl | grep 'disk0s2' | awk '{print $4}'
echo "Uptime: "`uptime | awk '{print "" $3 " " $4 " " $5 }' | sed -e 's/.$//g';`
</pre>

You might also be interested to check out other uses of Geektool on <a href="http://www.flickr.com/photos/tags/geektool/">Flickr</a>

To Apple: Let us put Widgets on the Desktop too please (without having to use a hack).
