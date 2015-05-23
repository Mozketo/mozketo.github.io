---
layout: post
title: Speed up Visual Studio 2010 (RAMDisk)
date: 2011-07-17 09:18
author: bclarkrobinson
comments: true
categories: [VisualStudio]
---
Visual Studio can at times be painfully slow. From startup, to compile, to running the web-application. Without having to resort to hardware solutions which can cost $$$ I investigated how to get Visual Studio running as fast as possible. This included tweaking VS options and using a RAMDisk.

*tl:dr* Using a RAMDisk may halve compile times (your milage may vary). SSDs are (in practice) close to the speed of a RAMDisk.

<table>
<thead>
<tr>
<th colspan="2">Solution Compile Times</th>
</tr>
<tr>
<th>7200rpm HDD</th>
<th>RAMDisk</th>
</tr>
</thead>
<tbody>
<tr>
<td>23 seconds</td>
<td>12 seconds</td>
</tr>
</tbody>
</table>

# Tweaking VS Options

First up I recommend reading this article from [Daniel Fisher](http://lennybacon.com/2010/10/18/UltimateGuideToSpeedUpVisualStudio.aspx). This will give some great tweaks to VS options and anti-virus exclusions to give your system a small speed boost (note: I prefer to leave 'Track active item in Solution Explorer' on).

# RAMDisk

Next up I tried setting up a RAMDisk to simulate having an SSD. A RAMDisk will take some of your system RAM and make it appear as a very fast drive on your system. The size of the RAMDisk will reduce the amount of free RAM. If you have 6GB RAM and the RAMDisk is 2GB, this will leave 4GB of available RAM for the OS and other processes.

To give you an idea of how fast your system RAM compared to a 7200RPM HDD I used a simple benchmarking tool (CrystalDiskMark).

<table>
<thead>
<tr>
<th colspan="3">HDD speeds in MB/s</th>
</tr>
<tr>
<th></th>
<th>7200rpm HDD</th>
<th>RAMDisk</th>
</tr>
</thead>
<tbody>
<tr>
<td>Read</td>
<td>105</td>
<td>4995</td>
</tr>
<tr>
<td>Write</td>
<td>85</td>
<td>3554</td>
</tr>
</tbody>
</table>

## Setting up RAMDisk

Download [RAMDisk](http://memory.dataram.com/products-and-services/software/ramdisk)>, install it and create a RAMDisk size ~1024MB. Configure it so it should automatically Load/Save the RAMDisk image. And I recommend turning on the AutoSave for approx ~15 minutes (900 seconds).

After you create the RAMDisk open the Windows Disk Management app and partition/format the new drive (NTFS, quick format) to drive R:.

I tried two different approaches to using the RAMDisk so I've separated these into "Phases". Phase 2 is slightly more complicated.

## RAMDisk - Phase 1

With the RAMDisk running create a folder on the drive R:\"tempvs".

{% highlight xml %}
r:
mkdir tempvs
{% endhighlight %}

Let's now tell Visual Studio to use this RAMDisk for it's temporary folder. Create a batch file vs2010.bat with the contents:

{% highlight xml %}
set TEMP=R:\tempvs
set TMP=R:\tempvs
"C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\devenv.exe"
{% endhighlight %}

Also tweak your web.config so ASP.NET dynamic compilation system can run as fast as it possibly can with the tempDirectory attribute:

{% highlight xml %}
&lt;compilation debug="true" strict="true" explicit="true" tempDirectory="R:\tempaspnet"&gt;
{% endhighlight %}

If there was a speed boost at this point it was negligable.

## RAMDisk - Phase 2

Now I wanted to see the compile time performance if I moved all the project source on to the RAMDisk too. By using a symlink from our current source location to our RAMDisk means there is no configuration changes or mucking around with the Solution or Project files.

_If 1024MB RAMDisk isn't big enough for your projects source code, resize it. (The existing RAMDisk will be lost and you have to create a new one)._

*Please backup your HDD or commit to source control before attempting this.*

Copy solution folder on to the RAMDisk. Example c:\source\client\project =&gt; r:\source\client\project.

Rename (or remove) the existing solution folder. Example c:\source\client\project =&gt; c:\source\client\project_before_ramdisk

Now create the symlink to the solution folder on the RAMDisk (I'm using Windows 7, unsure if the command line differs for Vista)

{% highlight xml %}
c:
cd \source\client\
mklink /d "project" "r:\source\client\project"
{% endhighlight %}

You will now have a symlink from c:\source\client\project =&gt; r:\source\client\project. When you start Visual Studio and open your project from c:\source\client\project (like normal) the project should just load. With luck it will feel snappier and your compile times will be faster.

## RAMDisk - Observations

Running a RAMDisk didn't appear to have the speed benefits that I would have originally expected. I think because the .NET compiler has been optimised into oblivion the speed benefits of the RAMDisk is minimal.

That said, halving the time your waiting on the compiler is a big win.

Also if you're running a SSD or RAID 0 solution on your PC already, I doubt there will be much benefit to you. Also if you're already using fast disks you're getting all-round speed benefits rather than just using a RAMDisk for source compilation.

So what speed difference did I observe?

<table>
<thead>
<tr>
<th colspan="2">Solution Compile Times</th>
</tr>
<tr>
<th>7200rpm HDD</th>
<th>RAMDisk</th>
</tr>
</thead>
<tbody>
<tr>
<td>23 seconds</td>
<td>12 seconds</td>
</tr>
</tbody>
</table>

Overall VS2010 feels snappier, and if you're compiling many times a day and/or frequently running your Unit Tests the difference of even a few seconds will add up over days/weeks/months.

Turning off the RAMDisk and returning to my previous set up feels sluggish compared, so I will keep with this solution for the time being.

##RAMDisk Warning

I want to be very clear that you're using a RAMDisk. This means if your computer loses power you will lose your changes! That is until the RAMDisk is written to its image or you've committed to SCM you are running a risk.

# Cheap Hardware Solutions

I also installed a 2nd HDD and moved the System Page file on to it as well as the SQL Server database MDF/LDF files.

I honestly can't think of much more, I'm clutching at straws on this one.

# Slightly more expensive Hardware Solutions

Upgrade to an SSD, 10,000RPM HDD or RAID 0 the HDDs. Not only will your compile times decrease, but you're also going to see speed benefits for IIS, Visual Studio start up times, etc.

Get as much RAM as your boss will allow (whether that be management or wife).

Throw as much processing power as you can afford at the problem. I'm running an i7 2.8GHz and wishing for more.

# Moving VS2010 on to RAMDisk

I created a 2GB RAMDisk and symlinked VS2010 onto it! I was hoping for a massive speed boost! Sadly this was not the case.

<table>
<thead>
<tr>
<th colspan="2">VS2010 Start Up Times</th>
</tr>
<tr>
<th>7200rpm HDD</th>
<th>RAMDisk</th>
</tr>
</thead>
<tbody>
<tr>
<td>1:15</td>
<td>0:43</td>
</tr>
</tbody>
</table>

As this computer has 6GB RAM, with a 2GB RAMDisk I'm now down to 4GB of available RAM. At this point I believe the amount of available RAM for VS is now to low and the system is starting to page. Perhaps if I had 8GB+ RAM this would be a viable solution.

# System Specs

Intel Core i7 at 2.8GHz
6GB RAM
7200RPM HDD

Sources: [lennybacon.com](http://lennybacon.com/2010/10/18/UltimateGuideToSpeedUpVisualStudio.aspx) [codewrecks.com](http://www.codewrecks.com/blog/index.php/2009/08/31/speedup-visual-studio-with-ramdisk/)
