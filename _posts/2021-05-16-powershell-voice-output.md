----
layout: post
title:  "Powershell voice output"
date:   2021-05-16 08:28:00 +0100
categories: powershell productivity de
----
# I'm Afraid I Can't Do That, Dave.
With this sentence, HAL 9000 supposedly ends the existence of its crew in the film _2001 - A Space Odyssey_.

But we are not in space and we are not working with an AI - only with Powershell.

I would like to show with a small example how a voice output from Powershell scripts is possible and how it can be used sensibly.

# When is the SQL job finished?
A fortnight ago I worked on a bug, the analysis of which required the repeated execution of jobs as well as several restores of a DB on a SQL server. The job ran for about 15 minutes and the restore operation about 20.

Now, of course, you can set a timer or refresh the job overview every two minutes in Management Studio,
but that's all overhead and distracts from things you can sensibly do on the side.

## Powershell script as an approach
First of all, I wanted to get rid of the constant switches to the SSMS and thus wrote a Powershell script with which I could trigger a job or recovery.

For the job it looks like this

```Powershell
Param
  (
    [parameter(Mandatory=$true)][string] $SQLServer,
    [parameter(Mandatory=$true)][string] $username,
    [parameter(Mandatory=$true)][string] $password,
    [parameter(Mandatory=$true)][string] $dbName,
    [parameter(Mandatory=$true)][string] $jobName
  )

$query = "SELECT top 1
yes.job_id,
j.name AS job_name,
yes.start_execution_date,      
ISNULL(last_executed_step_id,0)+1 AS current_executed_step_id,
Js.step_name
FROM msdb.dbo.sysjobactivity yes 
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

        Write-Host "Updated at: $(Get-Date -format 'u')" -ForegroundColor Red
        Write-Host "Status : $($jobName) is still running." -ForegroundColor Red

        Start-Sleep -Seconds 60
        continue
    }

    break;
}

Clear-Host
Write-Host "$(Get-Date -format 'u'): $($jobName) has finished" -ForegroundColor Green
```

The problem here is that a Powershell window can sometimes be minimised or hidden.
In these cases, I have to actively switch to the window again. Although the action for refreshing is omitted, it's still one step that can be saved.

## Powershell, talk to me
What options do I have to be notified by Powershell even if the window is currently in the background?

Sure, I could try to bring the window to the foreground, but that's a bit complicated (or my websearch foo isn't strong enough).

An alternative would be voice output. Powershell is so well integrated into Windows, it must be able to do that.
Of course it is [[1]](#1) and it only needs three lines of code at the end of the script:
```Powershell
# Create a new SpVoice objects
$voice = New-Object -ComObject Sapi.spvoice

# Set the speed - positive numbers are faster, negative numbers, slower
$voice.rate = 0

# Say something
$voice.speak("The SQL job $($jobName) has finished")
```
Depending on the name of the job, it takes some imagination to recognise the pronunciation, but the basic principle works.
This way I don't have to consciously change the context anymore, as long as it's not necessary.

---
Sources
<a name="1"></a>[1] [How do you get Windows Powershell to play a sound after .bat job has finished running?](https://stackoverflow.com/questions/56032478/how-do-you-get-windows-powershell-to-play-a-sound-after-bat-job-has-finished-ru)