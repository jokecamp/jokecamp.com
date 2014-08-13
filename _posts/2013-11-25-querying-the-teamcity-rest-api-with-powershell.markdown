---
author: jokecamp
comments: true
date: 2013-11-25 20:49:36+00:00
layout: post
slug: querying-the-teamcity-rest-api-with-powershell
title: Querying the TeamCity Rest API with Powershell
desc: "A powershell code example of querying the TeamCity REST Api to get the last successful build number."
wordpress_id: 1158
categories:
- Code
tags:
- automation
- continuous-integration
- devops
- powershell
- teamcity
---

The following powershell function demonstrates how to get the last successful TeamCity build number using the TeamCity REST API. This is a useful tool for continuous deployment to dev/test environments. I only cared about the build number but with some minor modifications of the XPath parsing you can get a lot of different information.

Requirements

  * REST API Plugin: Ensure you have the plugin installed. On the build server website go to
		`Administration > Server Administration > Plugins List`
		You should see the entry "REST API."

  * Authentication: You will need to either [enable guest access in TeamCity settings](http://confluence.jetbrains.com/display/TCD7/Enabling+Guest+Login) or first authenticate via the TeamCity API. Guest access allows a read-only view of our build server website and API.
  * Parameters: You will need to know your project id and TeamCity base url. You can determine the project id by seperate queries but for the simplicity of this example I have hard coded the id.

```powershell
<#
    Uses guest access to get the lastest successful build for a given project (project id is bt30)

    Guest Access needs to be enabled on TeamCity server or you will need to provide your
    own authentication.

    Example usage:
        Get-Latest-TeamCity-Build "http://teamcityserver/ "bt30" | Write-Output

#>
Function Get-Latest-TeamCity-Build([string]$baseUrl, [string]$projectId) {

    $url = "${baseUrl}guestAuth/app/rest/buildTypes/id:${projectId}/builds?status=SUCCESS"

    $xml = [xml](invoke-RestMethod -Uri $url -Method GET)
    $xpath = "/builds/build[1]"
    $latestBuild = Select-xml -xpath $xpath -xml $xml

    return @{
        "Build" = $latestBuild.Node.GetAttribute("id");
        "Revision" = $latestBuild.Node.GetAttribute("number");
        "Url" = $url
        }
}
```

[View the latest version of this function on GitHub](https://github.com/jokecamp/PowerShell.ToolBelt/blob/master/TeamCityFunctions.ps1).
