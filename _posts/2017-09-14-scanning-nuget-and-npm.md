---
layout: post
title: Scanning NuGet packages for vulnerabilities
date: 2017-09-14 10:23
author: bclarkrobinson
comments: true
categories: nuget security vulnerabilities vulnerability
share: y
---
During an internal development discussion we identified the trust we put into external NuGet packages this lead to implementing automated NuGet scanning using the [OWASP SafeNuGet](https://www.owasp.org/index.php/OWASP_SafeNuGet) package<sup>1</sup>.

This option allows developers to be advised early of issues or at build time in the build server (in our case VSTS).

Here's how to set it up:

### 1. Add SafeNuGet package

From Visual Studio click the _Tools > NuGet Package Manager > Package Manager Console_ menu item.

```
Install-Package SafeNuGet
```

Or if you have multiple projects:

```
Get-Project -All | Install-Package SafeNuGet
```

### 2. Analysis of Packages

By default the build will __fail__ if an issue was found otherwise you can can find the following in the _Build Output_ pane of Visual Studio:

```
Using cached list of unsafe packages
No vulnerable packages found
```

### 3. Options

If you'd prefer to not break the build you can configure the tool by editing the `./packages/SafeNuGet.<version>/build/SafeNuGet.targets` file and set the `DontBreakBuild` option to `true`.

> <sup>1</sup> _Update 14/Sep/2017_ After testing this process further I'm not 100% happy with it as it is _not_ triggering on known vulnerable NuGet packages (such as older JQuery packages). In my next post I investigate [DevAudit](https://github.com/OSSIndex/DevAudit) and [AuditJs](https://www.npmjs.com/package/auditjs).