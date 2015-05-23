---
layout: post
title: Speed up Visual Studio 2010 (RAMDisk)
date: 2011-07-17 09:18
author: bclarkrobinson
comments: true
categories: [Visual Studio]
---
&nbsp;

Visual Studio can at times be painfully slow. From startup, to compile, to running the web-application. Without having to resort to hardware solutions which can cost $$$ I investigated how to get Visual Studio running as fast as possible. This included tweaking VS options and using a RAMDisk.

<strong>tl:dr</strong> Using a RAMDisk may halve compile times (your milage may vary). SSDs are (in practice) close to the speed of a RAMDisk.

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

<h1>Tweaking VS Options</h1>
First up I recommend reading this article from <a href="http://lennybacon.com/2010/10/18/UltimateGuideToSpeedUpVisualStudio.aspx">Daniel Fisher</a>. This will give some great tweaks to VS options and anti-virus exclusions to give your system a small speed boost (note: I prefer to leave 'Track active item in Solution Explorer' on).
<h1>RAMDisk</h1>
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
<h2>Setting up RAMDisk</h2>
Download <a title="RAMDisk" href="http://memory.dataram.com/products-and-services/software/ramdisk" target="_blank">RAMDisk</a>, install it and create a RAMDisk size ~1024MB. Configure it so it should automatically Load/Save the RAMDisk image. And I recommend turning on the AutoSave for approx ~15 minutes (900 seconds).

After you create the RAMDisk open the Windows Disk Management app and partition/format the new drive (NTFS, quick format) to drive R:.

I tried two different approaches to using the RAMDisk so I've separated these into "Phases". Phase 2 is slightly more complicated.
<h2>RAMDisk - Phase 1</h2>
With the RAMDisk running create a folder on the drive R:\"tempvs".
<pre lang="dos" colla="+">r:
mkdir tempvs</pre>
Let's now tell Visual Studio to use this RAMDisk for it's temporary folder. Create a batch file vs2010.bat with the contents:
<pre lang="dos"colla="+">set TEMP=R:\tempvs
set TMP=R:\tempvs
"C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE\devenv.exe"</pre>
Also tweak your web.config so ASP.NET dynamic compilation system can run as fast as it possibly can with the tempDirectory attribute:
<pre lang="xml" colla="+">&lt;compilation debug="true" strict="true" explicit="true" tempDirectory="R:\tempaspnet"&gt;</pre>
If there was a speed boost at this point it was negligable.
<h2>RAMDisk - Phase 2</h2>
Now I wanted to see the compile time performance if I moved all the project source on to the RAMDisk too. By using a symlink from our current source location to our RAMDisk means there is no configuration changes or mucking around with the Solution or Project files.

<em>If 1024MB RAMDisk isn't big enough for your projects source code, resize it. (The existing RAMDisk will be lost and you have to create a new one).</em>

<strong>Please backup your HDD or commit to source control before attempting this.</strong>

Copy solution folder on to the RAMDisk. Example c:\source\client\project =&gt; r:\source\client\project.

Rename (or remove) the existing solution folder. Example c:\source\client\project =&gt; c:\source\client\project_before_ramdisk

Now create the symlink to the solution folder on the RAMDisk (I'm using Windows 7, unsure if the command line differs for Vista)
<pre lang="dos" colla="+">c:
cd \source\client\
mklink /d "project" "r:\source\client\project"</pre>
You will now have a symlink from c:\source\client\project =&gt; r:\source\client\project. When you start Visual Studio and open your project from c:\source\client\project (like normal) the project should just load. With luck it will feel snappier and your compile times will be faster.
<h2>RAMDisk - Observations</h2>
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
<h2>RAMDisk Warning</h2>
I want to be very clear that you're using a RAMDisk. This means if your computer loses power you will lose your changes! That is until the RAMDisk is written to its image or you've committed to SCM you are running a risk.
<h1>Cheap Hardware Solutions</h1>
I also installed a 2nd HDD and moved the System Page file on to it as well as the SQL Server database MDF/LDF files.

I honestly can't think of much more, I'm clutching at straws on this one.
<h1>Slightly more expensive Hardware Solutions</h1>
Upgrade to an SSD, 10,000RPM HDD or RAID 0 the HDDs. Not only will your compile times decrease, but you're also going to see speed benefits for IIS, Visual Studio start up times, etc.

Get as much RAM as your boss will allow (whether that be management or wife).

Throw as much processing power as you can afford at the problem. I'm running an i7 2.8GHz and wishing for more.
<h1>Moving VS2010 on to RAMDisk</h1>
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

<h1>System Specs</h1>
Intel Core i7 at 2.8GHz
6GB RAM
7200RPM HDD

Sources: <a title="lennybacon.com" href="http://lennybacon.com/2010/10/18/UltimateGuideToSpeedUpVisualStudio.aspx" target="_blank">lennybacon.com</a> <a title="codewrecks.com" href="http://www.codewrecks.com/blog/index.php/2009/08/31/speedup-visual-studio-with-ramdisk/" target="_blank">codewrecks.com</a>
