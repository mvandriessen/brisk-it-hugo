---
title: Get exclusions for all Veeam jobs
author: Maarten Van Driessen
type: post
date: 2016-10-10T13:38:30+00:00
url: /get-exclusions-veeam-jobs/
categories:
  - Powershell
tags:
  - powershell
  - Veeam

---
This will be another short one, but I figured someone else will have run into this.

While I was doing a rework for a Veeam implementation, I noticed on several jobs that there were exclusions set inside the jobs. I wanted a list of all jobs with their respective exclusions, time for Powershell!

The script starts by getting a list of all Veeam jobs. Next, it will go through all jobs and look for objects that have the type "Exclude" set. What follows is a bit of code to match the job name to the different exclusions and dump everything into a CSV. I struggled a bit with getting the contents of the array listed properly in the CSV, I kept getting the array listed as "_System Object[]_". Turns out I just needed to put the _$VMExclusions_ variable between quotes.

The CSV will look something like this, the job name is listed on the left and the excluded objects on the right.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/10/ExcludedVMs-CSV.png" target="_blank"><img loading="lazy" class="aligncenter size-full wp-image-115" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/10/ExcludedVMs-CSV.png?resize=561%2C122" alt="excludedvms-csv" width="561" height="122" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/10/ExcludedVMs-CSV.png?w=561&ssl=1 561w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/10/ExcludedVMs-CSV.png?resize=300%2C65&ssl=1 300w" sizes="(max-width: 561px) 100vw, 561px" data-recalc-dims="1" /></a>

One last note, this script needs to be run from the Veeam server itself.

As always, you can find the most recent version of the script on <a href="https://github.com/mvandriessen/Get-VeeamExclusions" target="_blank">Github</a>. The initial version can be found below.

<pre class="lang:ps decode:true ">Add-PSSnapin VeeamPSSnapin

$JobList = Get-VBRJob
$ExclusionList = @()
foreach($Job in $JobList)
{
    #Get exclusion per job
    $exclusions = $job.GetObjectsInJob() | Where-Object {$_.Type -eq "Exclude"}
    $VMExclusions = @()
    foreach ($exclusion in $exclusions)
    {
        $VMExclusions += ($exclusion.name)
    }
    
    $ExclusionList += New-Object -typeName psobject -Property @{
        Name = $job.Name
        ExcludedVMs = "$VMExclusions"
    }
}

$ExclusionList | Export-Csv ExcludedVMs.csv -delimiter ";" -NoTypeInformation</pre>

 

 