---
layout: post
title:  "Powershell voice output"
date:   2021-05-16 07:28:00 +0100
categories: powershell productivity de
---
# I'm Afraid I Can't Do That, Dave.
Mit diesem Satz beendet HAL 9000 vermeintlich die Existenz seiner Crew im Film _2001 - A Space Odyssey_.

Aber wir befinden uns nicht im All und arbeiten auch nicht mit einer KI sondern nur mit Powershell.

Ich möchte an einem kleinen Beispiel zeigen wie eine Sprachausgabe aus Powershell-Skripten möglich ist
und wie sie sinnvoll eingesetzt werden kann.

# Wann ist der SQL Job fertig?
Ich habe vor zwei Wochen einen Bug bearbeitet, dessen Analyse das wiederholte Ausführen von Jobs sowie
mehrere Wiederherstellungen einer DB auf einem SQL-Server erforderte. Der Job lief ca. 15 Minuten und 
die Restore-Operation ca. 20.

Jetzt kann man sich natürlich einen Timer stellen oder alle zwei Minuten im Management Studio die Job-Uebersicht aktualisieren,
aber das ist alles Overhead und lenkt von Sachen ab, die man sinnvoll nebenher erledigen kann.

## Powershell Skript als Ansatz
Als erstes wollte ich die dauernden Wechsel ins SSMS loswerden und habe ein Powershellskript geschrieben, mit dem 
ich Job bzw. Wiederherstellung antriggern kann.

Für den Job sieht das so aus

```powershell
Param
  (
    [parameter(Mandatory=$true)][string] $SQLServer,
    [parameter(Mandatory=$true)][string] $username,
    [parameter(Mandatory=$true)][string] $password,
    [parameter(Mandatory=$true)][string] $dbName,
    [parameter(Mandatory=$true)][string] $jobName
  )

$query = "SELECT top 1
ja.job_id,
j.name AS job_name,
ja.start_execution_date,      
ISNULL(last_executed_step_id,0)+1 AS current_executed_step_id,
Js.step_name
FROM msdb.dbo.sysjobactivity ja 
LEFT JOIN msdb.dbo.sysjobhistory jh ON ja.job_history_id = jh.instance_id
JOIN msdb.dbo.sysjobs j ON ja.job_id = j.job_id
JOIN msdb.dbo.sysjobsteps js
ON ja.job_id = js.job_id
AND ISNULL(ja.last_executed_step_id,0)+1 = js.step_id
WHERE
j.name = '$($jobName)'
AND start_execution_date is not null
AND stop_execution_date is null
order by start_execution_date desc"

while (1 -eq 1) {
    $result = Invoke-Sqlcmd -ServerInstance $SQLServer -Database $dbName -Query $query -Username $username -Password $password -Verbose

    if($null -ne $result -and $result.ItemArray.Length -gt 0 -and $result.ItemArray[1] -eq $jobName) {
        Clear-Host

        Write-Host "Aktualisiert um: $(Get-Date -format 'u')" -ForegroundColor Red
        Write-Host "Status         : $($jobName) wird immer noch ausgeführt." -ForegroundColor Red

        Start-Sleep -Seconds 60
        continue
    }

    break;
}

Clear-Host
Write-Host "$(Get-Date -format 'u'): $($jobName) ist beendet" -ForegroundColor Green
```

Das Problem hierbei ist, dass ein Powershell-Fenster eben auch mal minimiert oder verdeckt wird.
In diesen Fällen muss ich wieder aktiv zum Fenster wechseln. Zwar fällt die Aktion für das Aktualisieren weg,
aber es ist immer noch ein Schritt zu viel.

## Powershell, sprich mit mir
Welche Möglichkeiten habe ich, von Powershell benachrichtigt zu werden, auch wenn das Fenster gerade im Hintergrund ist?

Klar, ich könnte versuchen das Fenster in den Vordergrund zu bringen, aber das ist etwas kompliziert (oder mein Websearch-Foo ist nicht stark genug).

Eine Alternative wäre Sprachausgabe. Die Powershell ist so gut in Windows integriert, die muss das können.
Natürlich kann sie das [[1]](#1) und es braucht nur drei Zeilen Code am Ende des Skripts:

```powershell
# Create a new SpVoice objects
$voice = New-Object -ComObject Sapi.spvoice

# Set the speed - positive numbers are faster, negative numbers, slower
$voice.rate = 0

# Say something
$voice.speak("Der SQL Job $($jobName) ist beendet")
```
Je nach Name des Jobs braucht es etwas Fantasie die Aussprache zu erkennen, aber das Grundprinzip funktioniert.
Damit muss ich nicht mehr bewusst den Kontext wechseln, solange es nicht nötig ist.

<hr/>
Quellen

<a name="1"></a>[1] [How do you get Windows Powershell to play a sound after .bat job has finished running?](https://stackoverflow.com/questions/56032478/how-do-you-get-windows-powershell-to-play-a-sound-after-bat-job-has-finished-ru)