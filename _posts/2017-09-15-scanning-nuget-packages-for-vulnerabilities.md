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

### 3. AuditJS (NPM)

```
cd <path-with-package.json>
npm install auditjs -g
auditjs-win > auditjs-npm-projectname.txt
```