---
layout: post
title: Launch at Login Controller for your Mac Cocoa App
date: 2010-03-25 19:15
author: bclarkrobinson
comments: true
categories: iOS
---
Ever needed your Cocoa/Objective-C application to Launch at Login? And also have your app appear in your users System Preferences > Accounts > Login Items list? Want this integration to be super-mega-piss-easy?

Well now you can.

We're going to wrap LSSharedFileList to get our App to start at login - a Cocoa solution - as opposed to other solutions that use Carbon or having to edit XML system pref files.

Take a look at [Github - Launch at Login](http://github.com/Mozketo/LaunchAtLoginController) for the source. Don't forget to look at the readme for assistance for implementation.

Basically copy the LaunchAtLoginController.h/.m files into your project then either you can implement with Code or with Interface Builder.

## Code<

*Will app launch at login?*

<pre lang="objc" colla="+">
LaunchAtLoginController *launchController = [[LaunchAtLoginController alloc] init];
BOOL launch = [launchController launchAtLogin];
[launchController release];
</pre>

*Set launch at login state.*

<pre lang="objc" colla="+">
LaunchAtLoginController *launchController = [[LaunchAtLoginController alloc] init];
[launchController setLaunchAtLogin:YES];
[launchController release];
</pre>

## IB

* Open Interface Builder
* Place a NSObject (the blue box) into the nib window
* From the Inspector - Identity Tab (Cmd+6) set the Class to LaunchAtLoginController
* Place a Checkbox on your Window/View
* From the Inspector - Bindings Tab (Cmd+4) unroll the > Value item
* Bind to Launch at Login Controller
* Model Key Path: launchAtLogin

Easy right?

So what are you waiting for? Grab the code from [Github - Launch at Login](http://github.com/Mozketo/LaunchAtLoginController)
