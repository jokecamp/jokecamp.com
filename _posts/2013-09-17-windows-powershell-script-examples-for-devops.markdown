---
author: jokecamp
comments: true
date: 2013-09-17 20:58:38+00:00
layout: post
slug: windows-powershell-script-examples-for-devops
title: Windows Powershell Script Examples for devops
desc: "A listing of powershell scripts to help devops and for continuous integration."
wordpress_id: 1072
categories:
- Code
tags:
- devops
- powershell
---

Below is a list of useful scripts for devops and continuous integration. I will periodically update this post.

**Creating a multi-line bat file (or text file) via New-Item**

The following creates a bat file with commands to stop two services by name, a blank line for readability and then a command to run a Setup.exe program. This code could just as easily be used to create any type of plain text file.

```
$batFile = "e:\deployment\Install-rev-$rev.bat"
New-Item $batFile -type file -force

Add-Content $batFile "net stop `"Your Service 1`""
Add-Content $batFile "net stop `"Your Service 2`""
Add-Content $batFile ""
Add-Content $batFile "e:\deployment\Setup.exe"
```

**Providing Credentials via net-use**

```
net use \\server.domain.org\E$ "password" /USER:domain\jdoe
```


**Create EventLog Sources under a command Log Name via New-EventLog**
The below script must be executed as an Administrator since it will be modifying the registry. The script will iterate through an array of strings and create a new Event Log Source if it does not exist yet. If it exist an error will be shown and

```powershell
$sources = "Custom API", "Custom Web Service", "Custom Windows Service",

Foreach ($source in $sources) {
    Write-Host "Attempting to add '$source'"
    New-EventLog -LogName "MyApplicationFamilyName" -Source $source
}
```


**What version of PowerShell am I using?**

```
$PSVersionTable.psversion
```

Results in the following output:

```
Major  Minor  Build  Revision
-----  -----  -----  --------
3      0      -1     -1
```

[Source](http://stackoverflow.com/a/1825807/215502)
