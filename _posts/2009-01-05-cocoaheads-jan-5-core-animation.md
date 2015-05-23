---
layout: post
title: Cocoaheads Jan 5 - Core Animation
date: 2009-01-05 18:30
author: bclarkrobinson
comments: true
categories: [Core Animation, Talks, Xcode]
---
Today I did a little talk at Cocoaheads Brisbane, Australia. It was on Core Animation and I made a very very very very very simple application which would rotate a single image using <code>CATransform3DIdentity</code> to give it a nice perspective 3D rotation look.

Here's the full Xcode 3.1 project: <a href='http://mozketo.com/wp-content/uploads/2009/01/ca_photospin03.zip'>Download</a>. 

If you're interested solely in the transformation here's the code:

<pre lang="objc" colla="+">
float zDistance = 850;
CATransform3D sublayerTransform = CATransform3DIdentity;
sublayerTransform.m34 = 1.0 / -zDistance;
subLayer.transform = sublayerTransform;

CABasicAnimation *flipAnimation = [CABasicAnimation animationWithKeyPath:@"transform.rotation.y"];
flipAnimation.toValue = [NSNumber numberWithDouble:1.0f * M_PI];
flipAnimation.autoreverses = YES;
flipAnimation.duration = 2.0f;
flipAnimation.repeatCount = 1e100f;
flipAnimation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
[subLayer addAnimation:flipAnimation forKey:@"flip"];
</pre>

Where subLayer == your Core Animation layer you want to rotate.

Reference: <a href="http://www.pragprog.com/titles/bdcora/core-animation-for-mac-os-x-and-the-iphone">Core Animation for Mac OS X and the iPhone</a> and <a href="http://atomicwang.org/motherfucker/Index/B4E5D81C-A9A0-403C-B7A3-62FEB81DE777.html">Mike Lee</a>.
