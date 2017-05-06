---
author: Joe Kampschmidt
comments: true
layout: post
slug: powershell-script-update-webconfig
title: Simple Powershell script to replace values in a .net configuration file
desc: 'A simple powershell script to update a .net based xml web.config or app.config with environment specific values.'
tags:
- devops
- powershell
- xml
---

The following powershell script shows how to perform quick and dirty .net XML config file swapping. I use these quick swaps for our build server and continous deployments in Bamboo. This should work on Powershell version 2+.

There are two functions below:

 1. One for finding and replacing the key value pairs for AppSettings.
 2. Replacing the log4net RollingFileAppender filename.


```powershell

# swap a key/value in AppSettings
Function swapAppSetting {
  param([string]$key,[string]$value )
  $obj = $doc.configuration.appSettings.add | where {$_.Key -eq $key }
  $obj.value = $value
}

# swap the log4net filename
Function swaplog4Net {
  param([string]$value )
  $fileAppender = $doc.configuration.log4net.appender | where {$_.Type -eq 'log4net.Appender.RollingFileAppender' }
  $log = $fileAppender.file
  $log.value = $value
}


$webConfig = "\\server\path\app.config"
$doc = [Xml](Get-Content $webConfig)

swaplog4Net 'server-custom-name.log'
swapAppSetting 'env' 'test'

$doc.Save($webConfig)

```

Hopefully this saves you a few minutes.
