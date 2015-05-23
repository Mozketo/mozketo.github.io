---
layout: post
title: Bandwidth - Rackspace Cloud Server vs Rackspace Files
date: 2009-11-20 21:45
author: bclarkrobinson
comments: true
categories: [Rackspace, Uncategorized]
---
I've recently been looking at hosting a complex JBoss/Apache project at Rackspace Cloud as opposed to our own servers+pipe at work. 

I had changed the application to deliver the files from a CDN (in this case Rackspace Files) but thought "well Rackspace Cloud might be on the same pipe(s) as Files and could kill two birds with one stone".

Essentially I wanted to know the pipe difference between Rackspace <a href="http://www.rackspacecloud.com/cloud_hosting_products/servers">Cloud Servers</a> and Rackspace <a href="http://www.rackspacecloud.com/cloud_hosting_products/files">Files</a> and if Cloud was fast enough do away with Files.

<strong>The Test</strong>

Here's some <em>very</em> rudimentary results where I download a 9.6 MB zip file.

Rackspace Cloud - Average download speed: 335 kb/s - Time: 27 seconds.
Rackspace Files - Average download speed: 1154 kb/s - Time: 8 seconds.

I used <code>curl -O http://.../content.zip</code> for the download test.

<strong>Conclusions</strong>

Rackspace Cloud doesn't use the same "pipes" as Rackspace Files (I've never read that Rackspace claim, "Cloud is as fast as Files" I wasn't expecting the same results, just curious).

A dedicated CDN is the best way to go for delivery of large files quickly. (Rackspace Files uses the Limelight Network). I've heard rumours that Rackspace has Australian edge(s) and this seems to be the case.

I will still want to use Rackspace Files for large content delivery.

<strong>Misc</strong>

I use TPG ADSL2+ at home and never seen downloads faster than 1250-1300 kb/s. Using a 2.16GHz Macbook Pro.

At the time of writing: <a href="http://www.rackspacecloud.com/cloud_hosting_products/servers">Rackspace Cloud Server</a> is a cloud server hosting infrastructure. <a href="http://www.rackspacecloud.com/cloud_hosting_products/files">Rackspace Files</a> is a Content Delivery Network.

