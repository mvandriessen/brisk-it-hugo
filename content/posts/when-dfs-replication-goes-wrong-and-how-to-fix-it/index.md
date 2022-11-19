---
title: When DFS Replication goes wrong…And how to fix it
author: Maarten Van Driessen
type: post
date: 2016-03-08T13:23:16+00:00
url: /when-dfs-replication-goes-wrong-and-how-to-fix-it/
categories:
  - Windows

---

A while back a client of ours had some issues with DFS replication between 2 nodes. One of the members reported a backlog of over 1 million files!

Upon further investigation, I found the DFS database on that node got corrupted which caused it start the initial sync again.

We let the process run it's course over a couple of days but noticed the progress was incredibly slow. We were averaging about 3000 files / hour. After some quick math, it'd take about 16 days before the sync completed. And that's not taking into account anyone working on the file servers.

That is when I stumbled upon this very detailed [blog post](https://techcommunity.microsoft.com/t5/storage-at-microsoft/dfs-replication-in-windows-server-2012-r2-revenge-of-the-sync/ba-p/424807) by Ned Pyle. There he explains in great detail how the process can go faster, using a DFS database clone export and import.

## **DFS state of affairs**


One thing to pay attention to, you can't import a DFS database on a node that already has a pre-existing database. In order to make this strategy work, we had to make sure all traces of the existing DFS database were gone.

This isn't as simple as just removing the member and adding it again. When you delete a member from a replication group, that member isn't actually deleted from the database. The member is marked with a 30-day tombstone flag. If you change any files on that member and then re-add it, the tombstone flag is removed and the files on that member are used and replicated to the other members. You can imagine this can get really ugly if you decide to delete files on that "removed member". The key takeaway here is to make sure you have a backup before you make any changes!

We decided the easiest and safest way to go ahead was to delete all replication groups and recreate them. Simply removing the replication group isn't enough. You also have to make sure there are no remnants of the database in the "System Volume Information" folder. The easiest way to do this is by running these commands

```
  net stop dfsr
  icacls "D:\System Volume Information" /grant "Domain Admins":F
  cd "D:\System Volume Information"
  move DFSR D:\DFSR_backup
  icacls "D:\System Volume Information" /remove:g "Domain Admins"
  net start dfsr
```

This will move the contents of the folder to a DFSR_backup folder on the same volume. _Note: make sure you delete the replication groups before running these commands. If you don't do this, DFS will just recreate the database on that member._

## **Checking the nodes are in sync**
First, we had to make sure that any files that had changed on the remote node were copied to the node on the main site.
Sounds like a job for robocopy! I found out that the database got corrupted on the 12th of December, so we only had to check the files modified after that date.
The command looks like this

```robocopy \\serverb\share \\servera\share /e /XO /MAXAGE:20151211 /COPYALL /XF thumbs.db```
  

This command will compare all files newer than the 11th of December and copy them over. The /XF parameter excludes the thumbs.db file from being copied in order to speed up the process. Our client works with a lot of images so there are a lot of thumbs.db files.

## **Moving on**

To finish setting up your DFS-R, you can follow the steps outlined in the [blog post](http://blogs.technet.com/b/filecab/archive/2013/07/31/dfs-replication-in-windows-server-2012-r2-revenge-of-the-sync.aspx) I linked at the top of this page.


 