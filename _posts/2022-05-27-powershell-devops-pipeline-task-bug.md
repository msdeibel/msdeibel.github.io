---
layout: post
title:  "Bug in Powershell Copy-Item task"
date:   2022-05-27 20:13:00 +0100
categories: powershell azure devops
postLink: copyitembug
---
{% assign relatedPosts=site.documents | where:"postLink", page.postLink | sort: 'language' %}

<div class="language">
|
    {% for p in relatedPosts %}
      <a class="{{ p.language }}" href="{{ site.base-url }}{{ p.url }}">{{ p.language }}</a> |
    {% endfor %}
</div><br/>
<hr>
<br/>

**tl;dr**

Powershell's `Copy-Item -Path C:\parent\path\* -Recurse` in conjunction with the Azure DevOps _Powershell_ task has a nasty bug - workaround see below.

## The black Powershell swan

Yesterday I ran into a nasty Powershell bug that occurs in conjunction with the _Powershell_ task in Azure DevOps.

The ten lines in the Powershell task replace about 20 individual, less flexible, _copy-item_ tasks.

The use of the script is also necessary mainly because not all copy tasks have to be executed every time the pipeline is run.

The erroneous line follows the pattern

```powershell
Copy-Item -Path C:\source\path\* -Recurse -Destination C:\target\path\ -Force
```

This command normally (outside the DevOps task) copies the entire contents of `path` to the destination directory.

In the DevOps task, however, this does not happen because recursively determined directories are not created in the target path.

The bug is known and captured both [in the Azure Pipelines Repo](https://github.com/microsoft/azure-pipelines-tasks/issues/13456) and [in the Powershell Repo](https://github.com/PowerShell/PowerShell/issues/7005).

Since it has been open in the PS repo since 2018(!), it is unlikely to be fixed.

## Workaround

After finding the source of the error, I was able to implement a simple fix for the problem with 5 lines of code (all below the comment)

```powershell
$servicesToCopy = "comma,separated,list"

$servicesToCopy | ForEach-Object {
    $source = "$(PackageDirectory)\$($_ -replace '\s','')\drop"

    # Get source items individually since Copy-Item has a bug in connection with DevOps,
    # that makes it impossible to use \* for directory contents
    # see https://github.com/PowerShell/PowerShell/issues/7005
    $contentItems = gci $source
    
    $contentItems | ForEach-Object {
        Write-Host "Copy-Item -Path $($_.FullName) -Recurse -Destination $($destination) -Force -Verbose"

        Copy-Item -Path $($_.FullName) -Recurse -Destination $($destination) -Force -Verbose
    }
}
```

I really didn't expect such a bug in one of the most basic functions like `Copy-Item`.

But luckily it only took three quarters of a day to find and fix it.
