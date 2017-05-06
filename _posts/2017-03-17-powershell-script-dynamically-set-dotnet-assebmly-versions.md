---
author: Joe Kampschmidt
comments: false
layout: post
slug: powershell-script-dynamically-set-dotnet-assebmly-versions
title: Powershell script to dynamically set .NET Assembly/DLL versions for git
desc: 'A build server powershell script to dynamically set C# .NET Assembly and DLL versions using git tags for automated build dll versioning'
tags:
- devops
- powershell
---



How can you dynamically set a C# .net project assembly (dll) version attributes on a build server specifially for git version control?

A simple solution is to **just use a powershell script on the build server to change all the AssemblyVersion attributes on the fly right before compiling the solution**. This requires no special dev tooling or msbuild changes and you can utilize the popular practice of git tags for release/version tagging.

## .NET Project Versioning

Our script will target the following attributes in the `properties > AssemblyInfo.cs` file.

```csharp
[assembly: AssemblyVersion("0.0.0.0")]
[assembly: AssemblyFileVersion("0.0.0.0")]
```

## Powershell script details

Our script will:

1. Query git for the latest version tag and number of commits on the branch
2. Find all the `AssemblyInfo.cs` files and just use regex to swap in our versions
3. Build the solution

Some assumptions for this method:

- The version parser expects you have already tagged your git repo with a version. For example: `git tag -l "v2.0.0"` and be sure to push the tag.
- I use `git describe --long --tags --always` to get the most recent tag and the number of commits. I use the number of commits to easily track incremental changes for devs and QA.
- The regex expects either a 3 or 4 digit version number already exists in your `AssemblyInfo.cs` file.
- In Bamboo Build server make sure you are not using "Shallow Clones" for the repo. They will not include the git tags.

## Powershell script to dynamically change AssemblyInfo.cs Files

The script can easily be tailored to fit your needs. As a powershell amateur I just did the bare minimum to get the script working.

View as [Gist](https://gist.github.com/jokecamp/a2a314d62490fca1517a9a031c5606e9) or the code below:

```powershell

function getVersion()
{
    $tag = iex "git describe --long --tags --always"
    $a = [regex]"v\d+\.\d+\.\d+\-\d+"
    $b = $a.Match($tag)
    $b = $b.Captures[0].value
    $b = $b -replace '-', '.'
    $b = $b -replace 'v', ''
    Write-Host "Version found: $b"
    return $b
}


function SetVersion ($file, $version)
{

    "Changing version in $file to $version"
    $fileObject = get-item $file
    #$fileObject.Set_IsReadOnly($False)

    $sr = new-object System.IO.StreamReader( $file, [System.Text.Encoding]::GetEncoding("utf-8") )
    $content = $sr.ReadToEnd()
    $sr.Close()

    $content = [Regex]::Replace($content, "(\d+)\.(\d+)\.(\d+)[\.(\d+)]*", $version);

    $sw = new-object System.IO.StreamWriter( $file, $false, [System.Text.Encoding]::GetEncoding("utf-8") )
    $sw.Write( $content )
    $sw.Close()
    #$fileObject.Set_IsReadOnly($True)
}

function setVersionInDir($dir, $version) {

    if ($version -eq "") {
        Write-Host "version not found"
        exit 1
    }

    # Set the Assembly version
    $info_files = Get-ChildItem $dir -Recurse -Include "AssemblyInfo.cs" | where {$_ -match 'my.namespace'}
    foreach($file in $info_files)
    {
        Setversion $file $version
    }
}

# First get tag from Git
$version = getVersion
$dir = "./"
setVersionInDir $dir $version
```


### Other Solutions

 - Use a [MSBuild Community Task](http://stackoverflow.com/a/4970347/215502) that requires remembering to manually edit the `.csproj` file.