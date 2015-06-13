---
layout: post
title: TeamCity Slack Notifier
date: 2015-06-03 21:52
author: bclarkrobinson
comments: true
categories: slack
---
We're currently ~~playing with~~ slowly adopting Slack in our R&D department developers quickly raised a request to have TeamCity pump build failures into a Slack channel. Like this...

![Build failure notification example](/images/2015/06/teamcitybot-new-hotness.png)

<!--more-->

 Lucky for me I bumped across this [open Source project](https://github.com/enlivenhq/teamcity-slack) and being open source I was able fork the project and make some minor tweaks to the output.

So if you're looking a small TeamCity plugin that pushes Slack notifications out take you can download the latest [release here](https://github.com/Mozketo/teamcity-slack/releases), follow along with TeamCity installation instructions [here](https://github.com/Mozketo/teamcity-slack) (where you'll also find the code).

Plugin compatible with TeamCity 9.0.5.

## How does it look?

Here's an example of a build failure notification from Slack. The username & avatar is configurable via Slack Integrations, and the channel to send is configurable from the TeamCity plugin. 