---
title: Cannot get extent connection. Failed to restore file from local backup
author: Maarten Van Driessen
type: post
date: 2017-03-06T19:42:19+00:00
url: /cannot-get-extent-connection/
categories:
  - Veeam
tags:
  - Veeam

---
When I got into the office this morning, I noticed that on particular copy job hadn't done its\` job over the weekend. This particular job copies the daily restore points to a separate scale-out repository and enforces the GFS scheme that's been set.

The job report displayed

{{< figure src="./images/Veeam-replica-error.png" alt="veeam replica error">}}

Not that much to go on if you ask me. First, I checked to see if all extents in the repository still had enough room, this was the case. While I was doing that, I verified that all my proxies were still up and running. Before heading to my good friend Google, I decided to remove the copy job restore points from the configuration.

After this, I did a rescan of the repository and retried the job. It ran without a hitch, a nice and easy fix 🙂 I hope this won't become a common thing, time will tell.