---
author: Joe Kampschmidt
comments: true
layout: post
slug: invoke-restmethod-powershell-examples
title: Simple Examples of PowerShell's Invoke-RestMethod
desc: 'Consume a REST API with Windows PowerShell using the Invoke-RestMethod function with these practical GET, PUT, POST and DELETE examples.'
tags:
- rest
- powershell
- api
---


The [documentation for Invoke-RestMethod](http://technet.microsoft.com/en-us/library/hh849971.aspx) is a long sea of text. Skip it. These simple examples should get your started with consuming a REST API with PowerShell. Just a quick note that `Invoke-RestMethod` will parse the HTTP response for you and return a PowerShell object.


## Simple GET example

```powershell
$response = Invoke-RestMethod 'http://example.com/api/people'
# assuming the response was in this format { "items": [] }
# we can now extract the child people like this
$people = $response.items
```

## GET with custom headers example

```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("X-DATE", '9/29/2014')
$headers.Add("X-SIGNATURE", '234j123l4kl23j41l23k4j')
$headers.Add("X-API-KEY", 'testuser')

$response = Invoke-RestMethod 'http://example.com/api/people/1' -Headers $headers
```

## PUT/POST example

```powershell
$person = @{
    first='joe'
    lastname='doe'
}
$json = $person | ConvertTo-Json
$response = Invoke-RestMethod 'http://example.com/api/people/1' -Method Put -Body $json -ContentType 'application/json'
```

## DELETE example

```powershell
$response = Invoke-RestMethod 'http://example.com/api/people/1' -Method Delete
```
