---
layout: post
title: Migrating from TFS to git (VSTS)
date: 2017-08-21 15:08
author: bclarkrobinson
comments: true
categories: tfs git vsts visual-studio-team-services
share: y
---
The git-tfs project has some excellent documentation and step-by-step guides to migrate from TFS to git, I recommend you read here for more detail https://github.com/git-tfs/git-tfs

In summary here's the steps I used via the Windows 10 command line:

Peek into the branches available:

```
git tfs list-remote-branches https://<site:port>/tfs/<projectCollection>
```

If your TFS repo doesn't use branches you can use:

```
git tfs clone https://<site:port>/tfs/<projectCollection> $/<project> .
```

Otherwise if you want all the branches:

```
git tfs clone https://<site:port>/tfs/<projectCollection> $/<project> . --branches=all
```

This creates a local git repo from TFS.

From VSTS create a project or a new repo. Do not set a .gitignore file via the UI.

### Push to the server

```
git remote add origin <url>
git push -u origin master
or
git push --all origin
```

### Batch file template

```
@echo off
@setlocal

set serverpath=https://site/tfs/project
set tfsproject="$/projectname"
set repo="projectname"

git tfs list-remote-branches %serverpath%

if [%tfsproject%] == [] GOTO EXIT
if [%repo%] == [] GOTO EXIT

echo *** Cloning ***
if not exist %repo% mkdir %repo%
echo git tfs clone %serverpath% %tfsproject% %repo% --branches=all
git tfs clone %serverpath% %tfsproject% %repo% --branches=all
GOTO EXIT

:EXITEMPTY
echo tfsproject or repo variables are empty. Stopping here.

:EXIT
```

