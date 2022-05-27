---
layout: post
title:  "Copy-Item ist defekt"
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

Powershells `Copy-Item -Path C:\parent\path\* -Recurse` in Verbindung mit der Azure DevOps _Powershell_ Aufgabe hat einen fiesen Bug - Workaround siehe unten.

## Der schwarze Powershell Schwan

Gestern bin ich in einen üblen Powershell Bug gelaufen, der in Verbindung mit der _Powershell_ Task in Azure DevOps auftritt.

Die zehn Zeilen in der Powershell Aufgabe ersetzen ca. 20 einzelne, weniger flexible, _Copy-Item_ Aufgaben.

Die Verwendung des Skripts ist vor allem auch deshalb notwendig, weil nicht bei jeder Ausführung des Releases, alle Kopier-Aufgaben ausgeführt werden müssen.

Die fehlerhafte Zeile folgt dem Muster

```powershell
Copy-Item -Path C:\source\path\* -Recurse -Destination C:\target\path\ -Force
```

Dieser Befehlt kopiert normalerweise (außerhalb der DevOps Aufgabe) den gesamten Inhalt von `path` in das Zielverzeichnis.

In der DevOps Aufgabe passiert das aber nicht, weil rekursiv ermittelte Verzeichnisse im Zielpfad nicht angelegt werden.

Der Fehler ist bekannt und sowohl [im Azure Pipelines Repo](https://github.com/microsoft/azure-pipelines-tasks/issues/13456) als auch [im Powershell Repo](https://github.com/PowerShell/PowerShell/issues/7005) erfasst.

Da er im PS Repo seit 2018(!) offen ist, wird er wohl auch nicht mehr gefixt.

## Workaround

Nachdem die Fehlerquelle gefunden war, konnte ich mit 5 Zeilen Code (alles unter dem Kommentar) einen simplen Fix fuer das Problem implementieren

```powershell
$servicesToCopy = "komma,separierte,liste"

$servicesToCopy | ForEach-Object {

    $source = "$(PackageDirectory)\$($_ -replace '\s','')\drop"

    # Quellelemente einzeln holen da Copy-Item in Verbindung mit DevOps einen Bug hat,
    # der die Verwendung von \* fuer den Verzeichnisinhalt unmoeglich macht
    # s. https://github.com/PowerShell/PowerShell/issues/7005
    $contentItems = gci $source
    
    $contentItems | ForEach-Object {
        Write-Host "Copy-Item -Path $($_.FullName) -Recurse -Destination $($destination) -Force -Verbose"

        Copy-Item -Path $($_.FullName) -Recurse -Destination $($destination) -Force -Verbose
    }
}
```

Dass in einer der grundlegendsten Funktionen wie `Copy-Item` so ein Bug schlummert hätte ich wirklich nicht erwartet.

Hat aber zum Glück auch nur einen dreivirtel Tag gekostet, es zu finden und zu fixen.
