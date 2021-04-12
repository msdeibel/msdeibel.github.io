---
layout: post
title:  "Mandatory parameters with default values in Powershell"
date:   2021-03-12 07:40:42 +0100
categories: powershell coding en
---

## Comfort and functional security
This week I had a requirement to extend a powershell script. This was for a bug fix, but also to make the call more user-friendly.

Previously, the script processed a single file whose full path had to be provided by the user. However, the original request did not state that the user had to call the script for just under 200 files. And when I say "user", one immediately thinks of someone who doesn't necessarily know that he/she can relatively easily build a loop in Powershell that automates this.

So it was a matter of improving the call.

## Parameter protection in Powershell
After the content errors were worked off relatively quickly, it was time to work on the comfort functions.

However, securing the functionality must not suffer with the comfort. In this case, the script had to start without errors - even in an automated environment. This resulted in the following key points
- Provide the directory, instead of a single file as command line parameter.
- User query if no command line parameter is available
- Allow start with empty user input
- Allow paths with blanks

Parameters can already be secured to a certain extent with on-board means of Powershell. This includes, for example, typed, not-null-protected and mandatory parameters. Details can be found in Microsoft's own documentation on [about_parameters](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_parameters).

# The basis is inflexible
Unfortunately, the protection options are somewhat inflexible and do not fit the above scenario.

A parameter can be a mandatory
```powershell
param ([Parameter(Mandatory)]$servername)
```
but in this case you cannot assign a default value.
I.e. if the command line parameter is missing, you cannot simply set a default value as you can with
```powershell
param ($servername = 'server01')
```

Even a long search on the net led me to the same result over and over again: You have to program it yourself.

## Comfortable and secure
The code I ended up with consists of only a few lines that achieve the goal.

```powershell
Param ([string]$parameterCommandLine)

[string]$parameter = $parameterCommandLine

# If parameter is missing, request user input
if($parameter -eq "") {
  $parameter = (Read-Host -prompt "Enter parameter (empty - current directory")
}

# If user input is also empty, use current directory
if($parameter -eq "") {
  $parameter = '.\'
}

# escape blanks
$parameter -replace ' ', '` '

# make sure the path ends with \
if(!$parameter.EndsWith('\')) {
  $parameter = $parameter + '\'
}
```

With a few simple steps, we have a flexible scenario as far as parameter evaluation is concerned.