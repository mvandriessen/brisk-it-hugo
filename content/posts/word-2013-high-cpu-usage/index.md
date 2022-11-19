---
title: Word 2013 High CPU usage
author: Maarten Van Driessen
type: post
date: 2016-02-16T14:48:23+00:00
url: /word-2013-high-cpu-usage/
categories:
  - Office
tags:
  - Office 2013
  - Word

---
Late last week I had a client call me about Word acting slow. After a quick checkup, I noticed that Word was using 25 - 40% CPU. I hadn't received any other reports so I assumed it was a one-off and recreated the document. This sorted the issue for that particular computer.

Today I got the same report from a dozen more people. I started digging around in the documents that were having issues and noticed a drop in CPU usage when removing the header and footer. More specifically, removing images or objects in the header and footer.

Since I didn't have the issue on a freshly installed computer, I began suspecting updates were the cause. Sure enough, after removing KB3114717 everything went back to normal.

Microsoft has [pulled][1] the update already and advises people to uninstall it.

 [1]: https://support.microsoft.com/en-us/kb/3114717