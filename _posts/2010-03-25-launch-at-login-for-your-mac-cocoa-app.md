---
layout: post
title: Launch at Login Controller for your Mac Cocoa App
date: 2010-03-25 19:15
author: bclarkrobinson
comments: true
categories: [Uncategorized]
---
Ever needed your Cocoa/Objective-C application to Launch at Login? And also have your app appear in your users System Preferences > Accounts > Login Items list? Want this integration to be super-mega-piss-easy?

Well now you can.

We're going to wrap LSSharedFileList to get our App to start at login - a Cocoa solution - as opposed to other solutions that use Carbon or having to edit XML system pref files.

Take a look at <a href="http://github.com/Mozketo/LaunchAtLoginController">Github - Launch at Login</a> for the source. Don't forget to look at the readme for assistance for implementation.

Basically copy the LaunchAtLoginController.h/.m files into your project then either you can implement with Code or with Interface Builder.

<h2>Code</h2>

<em>Will app launch at login?</em>

<pre lang="objc" colla="+">
LaunchAtLoginController *launchController = [[LaunchAtLoginController alloc] init];
BOOL launch = [launchController launchAtLogin];
[launchController release];
</pre>

<em>Set launch at login state.</em>

<pre lang="objc" colla="+">
LaunchAtLoginController *launchController = [[LaunchAtLoginController alloc] init];
[launchController setLaunchAtLogin:YES];
[launchController release];
</pre>

<h2>IB</h2>
<ul>
	<li>Open Interface Builder</li>
	<li>Place a NSObject (the blue box) into the nib window</li>
	<li>From the Inspector - Identity Tab (Cmd+6) set the Class to LaunchAtLoginController</li>
	<li>Place a Checkbox on your Window/View</li>
	<li>From the Inspector - Bindings Tab (Cmd+4) unroll the > Value item</li>
        <li>Bind to Launch at Login Controller</li>
	<li>Model Key Path: launchAtLogin</li>
</ul>

Easy right?

So what are you waiting for? Grab the code from <a href="http://github.com/Mozketo/LaunchAtLoginController">Github - Launch at Login</a>
