---
layout: post
title: Installing Ruby on Rails on Ubuntu Karmic Koala
date: 2010-01-15 09:15
author: bclarkrobinson
comments: true
categories: [Ruby on Rails]
---
Here's the steps I took to install Ruby on Rails on a fresh Ubuntu 9.10 Karmic Koala.

<h3>Update/upgrade system</h3>
<code>sudo apt-get update</code>

<h3>Install ruby, irb and rdoc</h3>
<code>sudo apt-get install ruby irb rdoc</code>

<h3>Install required Ubuntu packages</h3>
<code>sudo apt-get install libopenssl-ruby build-essential ruby1.8-dev libpq-dev</code>

<h3>Install rubygems</h3>
<code>wget http://rubyforge.org/frs/download.php/60718/rubygems-1.3.5.tgz
tar -xvzf rubygems-1.3.5.tgz 
cd rubygems-1.3.5/
sudo ruby setup.rb
sudo gem update --system</code>

<h3>Install Rails (with gem)</h3>
<code>sudo gem install rails</code>

<h3>If you need to install postgresql</h3>
<code>sudo apt-get install postgresql</code>

Create the postgres user
<code>sudo su postgres
createuser <username></code>

Need to create a postgres database?
<code>createdb <db_name></code>

<h3>Install the ruby to postgres driver</h3>
<code>sudo gem install postgres</code>

<h3>Finished</h3>
That's it, Ruby on Rails should be installed along with Postgresql if you need it.

Credit goes to <a href="http://vandenabeele.com/Rails-on-Ubuntu-Jaunty">Peter Vandenabeele</a> for the basis for these steps.
