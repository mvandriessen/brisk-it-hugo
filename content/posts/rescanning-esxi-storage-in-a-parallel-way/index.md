---
title: Rescanning ESXi storage in a parallel way
author: Maarten Van Driessen
type: post
date: 2018-12-13T12:56:34+00:00
url: /rescanning-esxi-storage-in-a-parallel-way/
categories:
  - Powershell
  - VMware
tags:
  - powershell
  - VMware

---
Lately, I've been doing a lot of work provisioning and decommissioning LUNs on some Fibre Channel arrays. As you all know, this process can be somewhat tedious when provisioning a new LUN on vSphere or removing said LUN.

In the past, I would always use this PowerCLI command

```Get-VMhost -Cluster "Cluster1" | Get-VMHostStorage -Refresh```

This would first get all the ESXi hosts in the cluster "Cluster1" and then launch the refresh cmdlet on each host. The problem with this is that it can take a really long time for the command to complete because the refresh is executed host by host. Before starting the next refresh, the command waits for the previous one to finish. While this is fine for smaller environments, the time really does add up when you have a decent sized cluster.

## Using PowerShell jobs

In order to reduce the time this process takes, I decided this would be a great use case for PowerShell jobs. A job is essentially another PowerShell instance that runs in the background. This means that your main script or PowerShell window doesn't wait for it to finish before going to the next step. Using jobs will allow us to start the rescan on each host as a job and then start the next job. Meaning the rescans will happen in parallel.

## Code

The script is provided as is and can be found on [GitHub](https://github.com/mvandriessen/Invoke-RescanAllHBAGitHub)