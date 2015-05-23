---
layout: post
title: Why is CAConstraint not working?
date: 2009-02-02 09:46
author: bclarkrobinson
comments: true
categories: [CAConstraint, Cocoa, Core Animation]
---
So you're having problems with CAConstraint? It seems like no matter what you do the Constraint isn't being applied to the Sublayer? And you're CALayers are appearing in weird positions? Or not appearing at all?

Well the answer for me is surprisingly simple (and embarrassing)
<pre lang="objc" line="1" file="" colla="+">
[sublayer addConstraint:
[CAConstraint constraintWithAttribute:kCAConstraintMidX 
relativeTo:@"superlayer"
attribute:kCAConstraintMidX]]
</pre>

Now see that that @"superlayer"? Notice that it's not a Uppercase "L" as I was using "superLayer" where upon Core Animation was unable to find the layer "superLayer" and therefore have nothing to do with the constraints.
