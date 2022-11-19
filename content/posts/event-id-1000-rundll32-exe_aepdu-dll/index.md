---
title: Event ID 1000 rundll32.exe_aepdu.dll
author: Maarten Van Driessen
type: post
date: 2018-07-10T07:28:16+00:00
url: /event-id-1000-rundll32-exe_aepdu-dll/
categories:
  - Windows
tags:
  - Microsoft
  - Windows

---
While doing my morning check of our monitoring system I came across a strange issue...Two of our servers had been unavailable during the night, this had also happened the night before. Time to dig a bit deeper!

Both servers are domain controllers running Server 2012 R2, no updates were installed in the last few weeks and nothing had been changed either. Pulling up the event log of both servers, I found the exact same event logged on both machines at the time they were unavailable to our monitoring system.

```
  Faulting application name: rundll32.exe_aepdu.dll, version: 6.3.9600.17415, time stamp: 0x54504eb8
  Faulting module name: aeinv.dll, version: 6.3.9600.17415, time stamp: 0x54504e38
  Exception code: 0xc0000005
  Fault offset: 0x000000000003078b
  Faulting process id: 0x26b8
  Faulting application start time: 0x01d417ffa469e4cf
  Faulting application path: C:\WINDOWS\system32\rundll32.exe
  Faulting module path: C:\WINDOWS\system32\aeinv.dll
  Report Id: 32e94662-83f4-11e8-815d-0050568a21ba
  Faulting package full name: 
  Faulting package-relative application ID:
```

After I quick search, I found out that aeinv.dll is part of the Microsoft Customer Experience Improvement Program (CEIP). If you're signed up to the CEIP, which you are by default when you install Server 2012 R2, three scheduled tasks will run each night to upload your anonymized date to Microsoft.

  * AitAgent
  * Microsoft compatibility Appraiser
  * ProgramDataUpdater

We had the unavailability after the first and third task ran.

Luckily you can easily opt out of the CEIP by opening **Server Manager**, going to **Local Server** and clicking on the link behind Customer Experience Improvement Program.
{{< figure src="./images/participating.png" alt="participating">}}

There you will see that the radio button, **Yes, I want to participate in the CEIP **is selected. Click on **No, I don't want to participate** and click OK.

{{< figure src="./images/ceip.png" alt="ceip">}}

You're now opted out and the scheduled tasks will no longer run.

{{< figure src="./images/not-participating.png" alt="not-participating">}}

**UPDATE 11/07:** Turns out that just opting out of the CEIP does not actually prevent the scheduled tasks from running. In order to fully disable them, you need to disable them through the task scheduler or, even better, disable them through PowerShell

```Get-ScheduledTask -TaskPath "\Microsoft\Windows\Application Experience\" | Disable-ScheduledTask```