---
layout: post
title: TFS API - Track changeset merge in Branches
date: 2014-07-07 10:26
author: bclarkrobinson
comments: true
categories: TFS
---
This is a quick and easy way to identify what TFS paths a changeset has been merged into. Simply pass in the changeset ID, source path (like: $/project/dev) and a list of branches to check (like: $/project/release/release-1.0).

<!--more-->

{% highlight csharp %}
// Track a changeset merged into a possible list of branches.
public ExtendedMerge[] TrackChangesetIn(int changesetId, string projectPath, IEnumerable branches)
{
  var projectCollection = _tfsServer.Connection(); // Get your connection to TFS
  if (projectCollection.HasAuthenticated == false)
    projectCollection.Authenticate();

  // Get the Changeset list from the TFS API.
  var source = projectCollection.GetService();

  var merges = source.TrackMerges(new int[] { changesetId },
    new ItemIdentifier(projectPath),
    branches.Select(b =&gt; new ItemIdentifier(b)).ToArray(),
    null);

  return merges;
}
{% endhighlight %}

You can use this method like this:

{% highlight csharp %}
var mergeBranch = TrackChangesetIn(id, "$/project/dev", new List { "B1", "B2" });
if (mergeBranch.Any())
{
  var targetItems = mergeBranch.Select(mb =&gt; mb.TargetItem.Item);
}
{% endhighlight %}
