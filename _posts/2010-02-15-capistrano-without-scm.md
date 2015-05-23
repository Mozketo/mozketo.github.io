---
layout: post
title: Capistrano without SCM
date: 2010-02-15 21:39
author: bclarkrobinson
comments: true
categories: [Ruby on Rails]
---
I am unable to get Capistrano to deploy from our Subversion repository as it's on a local IP, with no VPN access or access from the outside world and I'm also not in a position to open up the SVN box to the outside world.

So how does Capistrano get access to the source code? 

Turns out it's not that hard, you just need to know the tricks. So open up your /config/deploy.rb file and add/modify the following lines:

<pre lang="Text" colla="+">
set :scm, :none
set :respository, "."
set :deploy_via, :copy
</pre>

<h2>Using Capistrano</h2>

Here's my notes for getting Capistrano up and running. You need only run these steps the first time: 

<pre lang="BASH" colla="+">
sudo gem install Capistrano
cd /your/project/directory
capify .
</pre>

copy deploy.rb (listed below) over project/config/deploy.rb

<pre lang="BASH" colla="+">
cap deploy:setup
cap deploy:cold
</pre>

Subsequent Capistrano usage needs only:

<pre lang="BASH" colla="+">
cap deploy
</pre>

<h2>deploy.rb</h2>

Below is the deploy.rb I've used (many thanks to <a href="http://twitter.com/aussiegeek">aussiegeek on twitter</a>). You might not require the first line <code>default_run_options[:pty] = true</code> I had to add it for use on Ubuntu 9.10.

Also I'm using Phusion Passenger.

<pre lang="Text" colla="+">
default_run_options[:pty] = true

set :application, 'projectName'
set :deploy_to, '/server/path/'
set :user, 'username'
set :use_sudo, false

role :web, "server.com"
role :db, "server.com", :primary => true
role :app, "server.com"

set :scm, :none
set :repository, "."
set :deploy_via, :copy

namespace :deploy do
  task :start, :roles => :app do
  end

  task :stop, :roles => :app do
  end

  task :restart, :roles => :app do
    run "touch #{current_path}/tmp/restart.txt"
  end
end
</pre>
