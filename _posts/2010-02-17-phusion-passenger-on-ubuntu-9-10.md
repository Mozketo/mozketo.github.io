---
layout: post
title: Phusion Passenger on Ubuntu 9.10
date: 2010-02-17 13:58
author: bclarkrobinson
comments: true
categories: [Ruby on Rails]
---
At first I was thrown by the Phusion Passenger installation instructions when it talked about enabling the Passenger module in apache .conf file. In Ubuntu the enabling/disabling of modules does not sit in a .conf file but, in fact, split into individual .load/.conf files in the /etc/apache2/mods-available folder.

This means when you enable a module with <code>a2enmod</code> in Ubuntu it simply makes a symlink from mods-enabled -> mods-available and disabling a module (with <code>a2dismod</code> is just as easy. No more hunting in those .conf files.

So I wanted to keep things using the same idea of using the mods-enabled directory as opposed to using one large apache2.conf file.

<h2>Phusion Passenger Installation</h2>
Passenger is drop dead simple to install, here's the steps.

Update the system
<pre lang="BASH" colla="+">
sudo apt-get update
sudo apt-get install apache2-prefork-dev
</pre>

Now Install Phusion Passenger
<pre lang="BASH" colla="+">
sudo gem install passenger
sudo passenger-install-apache2-module
</pre>

Passenger is now installed; let's get it enabled.

<h2>Create the .load/.conf in mod-available</h2>

Create the .load file with <code>sudo nano /etc/apache2/mods-available/passenger.load</code> and paste in the following:

<pre lang="Text" colla="+">
LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-2.2.9/ext/apache2/mod_passenger.so
</pre>

Note: The version numbers I've used might not be what you have installed; Passenger will give you the correct version numbers on screen once installation is complete.

Create the .conf file with <code>sudo nano /etc/apache2/mods-available/passenger.conf</code> and paste in the following:

<pre lang="Text" colla="+">
<IfModule passenger_module>
   PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-2.2.9
   PassengerRuby /usr/bin/ruby1.8
</IfModule>
</pre>

<h2>Enable Passenger</h2>

Enabling Passenger is as simple as:

<pre lang="BASH" colla="+">
sudo a2enmod passenger
sudo /etc/init.d/apache2 restart
</pre>

And you will need to change your <code>/etc/apache2/sites-available/default</code> to read:

<pre lang="Text" colla="+">
<VirtualHost *:80>
    ServerName www.rackapp.com
    DocumentRoot /webapps/rackapp/public
</VirtualHost>
</pre>

And you're done. You're up and running with Phusion Passenger.

You can read more about configuring Passenger <a href="http://www.modrails.com/documentation/Users%20guide.html">here</a>
