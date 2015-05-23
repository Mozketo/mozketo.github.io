---
layout: post
title: Using ffmpeg for BlackBerry Bold playback
date: 2010-02-02 13:59
author: bclarkrobinson
comments: true
categories: [Uncategorized]
---
Sometimes ffmpeg just doesn't play like you'd hope it would. I was trying to re-encode video clips for a BlackBerry Bold (the Bold supports H.264 playback) but every encode would have a blank black screen for video, and the audio would be fine (more on audio later).

I'm currently using SVN release r19352 (and also tested with r21566 (2010-01-31)).

<h3>ffmpeg arguments for BlackBerry H.264</h3>

<pre lang="Bash" colla="+">ffmpeg -v 0 -y -i input.mp4 -f mp4 -aspect 2.409 -vcodec libx264 -vpre default -vpre baseline -s 480x200 -r 24 -b 220k -acodec libmp3lame -ab 24kbit/s -ac 1 output.mp4</pre>

So lets break down these arguments:

<pre lang="Text" colla="+">
-v 0 = Set verbosity level
-y = Overwrite existing file
-i input.mp4 = The file you want to convert
-f = Force the video type
-aspect = The magical number to get the resize right
-vcodec libx264 = We're going to use x264 for video codec
-vpre default
-vpre baseline = Magical presets. And ensure BlackBerry compatibility
-s = The final dimensions of the clip
-r = Video frame rate
-b = Bitrate per second (the larger number the better the video)
-acodec libmp3lame = See Audio heading below
-ab = Audio bitrate
-ac 1 = Number of Audio Channels
output.mp4 = The output file
</pre>

<h3>-vpre isn't working for me (and I'm on Windows)</h3>

ffmpeg isn't a Windows only application and looks for its presets in <code>/usr/local/share/ffmpeg/</code> so if you create a folder structure on your Windows computer <code>c:\usr\local\share\ffmpeg\</code> and copy all the *.ffpreset files into said folder you won't have problems with -vpre anymore

<h3>Audio issues (AAC)</h3>

In later releases of ffmpeg AAC (or libfaac) was disabled; I assume due to AAC being patent encumbered. There's ways around turn libfaac back on, or you could just switch to using libmp3lame or the like.

