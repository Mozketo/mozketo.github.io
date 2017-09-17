---
layout: post
title: Scanning NuGet packages and NPM for vulnerabilities
date: 2017-09-15 11:21
author: bclarkrobinson
comments: true
categories: nuget security vulnerabilities vulnerability npm vsts
share: y
---
During an internal development discussion we identified the trust we put into external NuGet and NPM packages, this lead to implementing NuGet scanning using  [DevAudit](https://github.com/OSSIndex/DevAudit) and [AuditJs](https://www.npmjs.com/package/auditjs). 

Regarding automated scanning we're currently we're using Visual Studio Team Services (VSTS) hosted build agents which do not allow for custom applications to be installed. If you're using a self-hosted VSTS build agent you will be able to automate this at build time.

Previously I tried out OWASP SafeNuGet and found that the results were not as comprehensive as DevAudit.

### 1. DevAudit (NuGet)

DevAudit offers a lot of fuctionality, my use-case is only for NuGet scanning (for now). I used Chocolatey to install:

```
choco install devaudit
refreshenv
cd <path-with-package.config>
devaudit nuget -note-non-interact > devaudit-nuget-projectname.txt
```

A sample of the result from NuGet scan:

```
...
[35/49] Microsoft.AspNet.WebPages (3.2.3) no known vulnerabilities. 
[36/49] jQuery (1.10.2) Error determining vulnerability version range (>=1.4.0 <=1.11.3) | (>=1.12.4 <3.0.0-beta1) in package version range 1.10.2: Parsing failure: unexpected '('; expected <= or >= or < or > or = or ~ or digit (Line 1, Column 1); recently consumed: .
[VULNERABLE]
7 known vulnerabilities, 2 affecting installed version. 
...
```

### 3. AuditJS (NPM)

```
cd <path-with-package.json>
npm install auditjs -g
auditjs-win > auditjs-npm-projectname.txt
```

A sample of the result from AuditJs

```
...
[2/702] @types/draft-js 0.10.12   No known vulnerabilities...
[3/702] immutable 3.8.1   No known vulnerabilities...
...
```

Hopefully if you're reading this you're taking a proactive approach and for that I congratulate you.

Happy scanning :)