---
layout: post
title: MSBuild Community Tasks XmlRead and XmlUpdate
date: 2013-03-01 08:50
author: bclarkrobinson
comments: true
categories: msbuild
---
Importing the MSBuild Community Tasks into a Visual Studio project can make difficult tasks easy. In my scenario I wanted to hook into the BeforeBuild target so I could modify the defaultVersion attribute in a combres.xml before TeamCity created a Release build.

<!--more-->

Firstly you need to import the MSBuild Community Tasks (MSBCT) into the .csproj. And in my scenario I didn't want other developers to have to install MSBCT so I conditionally import on non-debug builds.

{% highlight xml %}
<Import Project="$(MSBuildExtensionsPath)\MSBuildCommunityTasks\MSBuild.Community.Tasks.Targets" Condition="'$(Configuration)' != 'Debug'" />
{% endhighlight %}

Then I wanted to prove the concept so I used XmlRead first. I had to workout the XPath first, so once that was defined I dropped that into the XPath attribute:

{% highlight xml %}
<XmlRead XmlFileName="App_Data\combres.xml" XPath="//*[local-name()='resourceSets']/@defaultVersion">             
  <Output TaskParameter="Value" PropertyName="combresDefaultValue" />
</XmlRead>
{% endhighlight %}

Running from the command line: <pre>msbuild.exe project.csproj /t:BeforeBuild /p:configuraton=Release</pre> I could see the output of XmlRead on the command line.

Switching to XmlUpdate now made sense. So I commented out the XmlRead and added 

{% highlight xml %}
<XmlUpdate XPath="//*[local-name()='resourceSets']/@defaultVersion"
             XmlFileName="App_Data\combres.xml"
             Value="$([System.DateTime]::Now.ToString(`yyyyMMddHHmmss`))" />
{% endhighlight %}

Now the combres.xml file has it's defaultVersion attibute modified for every Release build.

{% highlight xml %}
<resourceSets url="~/combres.axd" defaultVersion="20130228144417" ...>
{% endhighlight %}

