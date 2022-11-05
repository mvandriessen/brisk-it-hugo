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

<pre class="lang:default decode:true ">Faulting application name: rundll32.exe_aepdu.dll, version: 6.3.9600.17415, time stamp: 0x54504eb8
Faulting module name: aeinv.dll, version: 6.3.9600.17415, time stamp: 0x54504e38
Exception code: 0xc0000005
Fault offset: 0x000000000003078b
Faulting process id: 0x26b8
Faulting application start time: 0x01d417ffa469e4cf
Faulting application path: C:\WINDOWS\system32\rundll32.exe
Faulting module path: C:\WINDOWS\system32\aeinv.dll
Report Id: 32e94662-83f4-11e8-815d-0050568a21ba
Faulting package full name: 
Faulting package-relative application ID:</pre>

After I quick search, I found out that aeinv.dll is part of the Microsoft Customer Experience Improvement Program (CEIP). If you're signed up to the CEIP, which you are by default when you install Server 2012 R2, three scheduled tasks will run each night to upload your anonymized date to Microsoft.

  * AitAgent
  * Microsoft compatibility Appraiser
  * ProgramDataUpdater

We had the unavailability after the first and third task ran.

Luckily you can easily opt out of the CEIP by opening **Server Manager**, going to **Local Server** and clicking on the link behind Customer Experience Improvement Program.<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_24-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png" target="_blank" rel="noopener"><img loading="lazy" class="aligncenter wp-image-236 size-full" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_24-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=328%2C30" alt="" width="328" height="30" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_24-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?w=328&ssl=1 328w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_24-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=300%2C27&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_24-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=315%2C30&ssl=1 315w" sizes="(max-width: 328px) 100vw, 328px" data-recalc-dims="1" /></a>

There you will see that the radio button, **Yes, I want to participate in the CEIP **is selected. Click on **No, I don't want to participate** and click OK.

<img loading="lazy" class="aligncenter wp-image-235 size-full" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_32-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=552%2C397" alt="" width="552" height="397" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_32-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?w=552&ssl=1 552w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_32-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=300%2C216&ssl=1 300w" sizes="(max-width: 552px) 100vw, 552px" data-recalc-dims="1" /> 

You're now opted out and the scheduled tasks will no longer run.

<img loading="lazy" class="aligncenter wp-image-237 size-full" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_11-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=350%2C30" alt="" width="350" height="30" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_11-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?w=350&ssl=1 350w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2018/07/2018-07-10-08_57_11-remsup-rdp.rdremsup.local-Remote-Desktop-Connection.png?resize=300%2C26&ssl=1 300w" sizes="(max-width: 350px) 100vw, 350px" data-recalc-dims="1" /> 

**UPDATE 11/07:** Turns out that just opting out of the CEIP does not actually prevent the scheduled tasks from running. In order to fully disable them, you need to disable them through the task scheduler or, even better, disable them through PowerShell

<pre class="lang:ps decode:true  ">Get-ScheduledTask -TaskPath "\Microsoft\Windows\Application Experience\" | Disable-ScheduledTask</pre>