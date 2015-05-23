---
layout: post
title: IronRuby in Visual Studio - Console Tips
date: 2011-02-09 08:51
author: bclarkrobinson
comments: true
categories: [Visual Studio]
---
These are more a collection of IronRuby 1-liners for prosperity (ie my poor memory). I like to use IronRuby to be able to execute small snippets of code without having to run up a whole VS solution/project.

## Using IronRuby Interactive in Visual Studio 2010

Install IronRuby from CodePlex, download from [here](http://ironruby.codeplex.com/)
After installation open Visual Studio and using the menu: View > Quick Access; in the search box type 'Iron'.
Click IronRuby Interactive.

**DateTime**

{% highlight ruby %}
System::DateTime.UtcNow.AddMinutes(-10)
System::DateTime.UtcNow.Subtract(System::TimeSpan.from_minutes(5))
{% endhighlight %}