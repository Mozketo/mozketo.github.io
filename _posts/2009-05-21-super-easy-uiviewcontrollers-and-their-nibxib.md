---
layout: post
title: Super easy UIViewControllers and their nib/xib
date: 2009-05-21 21:27
author: bclarkrobinson
comments: true
categories: [Uncategorized]
---
I don't like initialising my UIViewControllers and their associated nib explicitly using a string

<pre lang="objc" colla="+">FlipsideViewController *viewController = [[FlipsideViewController alloc]
 initWithNibName:@"FlipsideView" bundle:nil];</pre>

But would rather my UIViewControllers know how to load themselves. This reduces the possibilities of typos, allows for only 1 place in code where your nib is referenced by name, and just looks better.

For example in <em>FlipsideViewController.m</em> I would add:

<pre lang="objc" colla="+">
- (id)initWithNibName:(NSString *)nibNameOrNil 
bundle:(NSBundle *)nibBundleOrNil {
  if ((self = [super initWithNibName:@"FlipsideView" bundle:nibBundleOrNil]))  {
    // Initialization code		
  }
  return self;
}
</pre>

Now you can simply initialise the ViewController using

<pre lang="objc" colla="+">FlipsideViewController *viewController = [[FlipsideViewController alloc] 
init];</pre>
