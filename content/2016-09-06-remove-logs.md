---
title: Remove Logs
author: Maarten Van Driessen
type: post
date: 2016-09-06T12:16:46+00:00
url: /remove-logs/
categories:
  - Powershell
  - Windows
tags:
  - function
  - logs
  - powershell

---
A couple of our web servers were running into some issues with disk space. Turns out the logs weren't being cleaned up properly.  
In order to remediate this, I wrote a function that can be reused anywhere.

The function accepts 3 parameters:

  * **FilePath: **The directory where you want to remove the logs from
  * **CutOff; **Specifies the age (in days) that a file must have before being deleted.
  * **LogPath; **This parameter specifies the directory where you want the CSV log file. If this is left open, no CSV will be saved.

## Example

As an example, I'm going to remove all files, older than 30 days, from the folder "C:\temp\W3SVC2030036971\W3SVC2030036971\". I want a CSV log to be written in the "C:\temp" folder.

<div id="attachment_104" style="width: 310px" class="wp-caption aligncenter">
  <a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs1.png"><img aria-describedby="caption-attachment-104" loading="lazy" class="wp-image-104 size-medium" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs1-300x208.png?resize=300%2C208" alt="remove-logs1" width="300" height="208" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs1.png?resize=300%2C208&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs1.png?w=729&ssl=1 729w" sizes="(max-width: 300px) 100vw, 300px" data-recalc-dims="1" /></a>
  
  <p id="caption-attachment-104" class="wp-caption-text">
    A view from the Powershell window
  </p>
</div>

As you can see, you always get feedback in your window, even if you don't specify a log path.

The CSV looks something like this:

[<img loading="lazy" class="aligncenter size-medium wp-image-105" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs2-128x300.png?resize=128%2C300" alt="remove-logs2" width="128" height="300" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs2.png?resize=128%2C300&ssl=1 128w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs2.png?w=311&ssl=1 311w" sizes="(max-width: 128px) 100vw, 128px" data-recalc-dims="1" />][1]

## Code

You can find the script code below. This is provided as-is. You can find the most recent version of the code on [GitHub][2].

<pre class="lang:ps decode:true">#Requires -Version 3.0

Function Remove-Logs{ 

 <#
.SYNOPSIS
  Delete old log files
.DESCRIPTION
  This function will delete all .log and .txt files older than a certain number of days.
  You have the ability to specify log path, extension (log or txt) and path.
.NOTES
  Author:  Maarten Van Driessen
.PARAMETER FilePath
  Specify the path where you want to delete the logs from.
.PARAMETER CutOff
  Specify how many days you want to go back.
.PARAMETER LogPath
  Where do you want to store the log file. Path must end with a \
.PARAMETER FileExtension
  Specify whether you want to delete *.log, *.etl or *.txt files. If left blank, the function will delete both.
  Must be written as *.log, *.etl or *.txt
.EXAMPLE
  Remove-Logs -FilePath C:\inetpub\ -Cutoff 30 -LogPath c:\temp\
  
  Delete all txt and log files older than 30 days from the c:\inetpub folder and write the log to c:\reports
.EXAMPLE
  Remove-Logs -FilePath C:\inetpub\ -Cutoff 20 -LogPath c:\temp\ -Filter *.txt
  
  Delete all txt files older than 20 days from the c:\inetpub folder and write the log to c:\reports
# <

[Cmdletbinding()]
#Parameters
Param
(
    #Check to see if there are any invalid characters in the path
    [Parameter(Mandatory=$true)][ValidateScript({
            If ((Split-Path $_ -Leaf).IndexOfAny([io.path]::GetInvalidFileNameChars()) -ge 0) {
                Throw "$(Split-Path $_ -Leaf) contains invalid characters!"
            } Else {$True}
        })][string]$FilePath,
    #You can only delete logs older than 1 day
    [Parameter(Mandatory=$true)][ValidateRange(1,365)][int]$Cutoff,
    #Check to see if there are any invalid characters in the path
    [ValidateScript({
            If ((Split-Path $_ -Leaf).IndexOfAny([io.path]::GetInvalidFileNameChars()) -ge 0) {
                Throw "$(Split-Path $_ -Leaf) contains invalid characters!"
            } Elseif(-Not ($_.EndsWith('\'))){
                Throw "Logpath must end with \ !"
            } Else {$True}
        })]$LogPath="",  
    
    #you can only delete log and txt files
    [ValidateSet("*.log","*.txt","*.etl")][string]$FileExtension ="*.*"
    
)

    #Check if the file & log path exist
    if(-not (Test-Path $FilePath))
    {
        throw "Invalid file path! Please make sure you enter a valid path."
    }
    #If statement to handle optional parameter
    elseif($LogPath -eq "") {}
    elseif(-not (Test-Path $LogPath))
    {
        throw "Log path does not exist! Please make sure you enter a valid path."
    }

    $CutOffDate = (Get-Date).AddDays(-$Cutoff)

    #Get all items inside the path
    $LogFiles = Get-ChildItem -Path $FilePath -Filter $FileExtension | Where-Object{$_.LastWriteTime -lt $CutOffDate}
    $DeletedItems = @()
    if($LogFiles)
    {
        foreach($LogFile in $LogFiles)
        {
            #Remove all items
            $DeletedItems += $LogFile
            $LogFile | Remove-Item
            Write-Host "Removing file $LogFile" -ForegroundColor Green
        }
        if($LogPath)
        {
            $LogName = Get-Date -Format "yyyy-MM-dd"          
            $DeletedItems | select Name,creationtime,lastwritetime | Export-Csv -path "$LogPath\DeletedLogs - $LogName.csv" -Delimiter ";" -NoTypeInformation -Append
        }
    }
    else 
    {
        Write-Host "No files were found." -ForegroundColor Red
    }
}</pre>

 [1]: https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/09/remove-logs2.png
 [2]: https://github.com/mvandriessen/Remove-Logs