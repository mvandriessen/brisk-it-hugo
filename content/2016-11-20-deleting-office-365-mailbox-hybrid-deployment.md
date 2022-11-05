---
title: Deleting an Office 365 mailbox in hybrid deployment
author: Maarten Van Driessen
type: post
date: 2016-11-20T15:21:22+00:00
url: /deleting-office-365-mailbox-hybrid-deployment/
categories:
  - Office 365
tags:
  - office365

---
When running Office 365 in a hybrid deployment, it is possible to have a mailbox both on premises as on Office 365. This can happen when you assign the user an Exchange Online license before the mailbox has been migrated to Office 365.

If the user's outlook is still configured to use the on-premises mailbox, this can create some funky issues. For example, sent items will be in the on-premises mailbox but new items will arrive in Office 365.

## Verifying the mailbox on Office 365

The first thing you have to do is verify if the user has a mailbox on 365, Powershell lets us do this. This command returns all

<pre class="lang:ps decode:true ">Connect-MsolService

Get-MsolUser | where-object {$_.userprincipalname -like "*user*"}</pre>

## Unsyncing user from Office 365

Once you've identified the user, that has a mailbox in Office 365, you have to remove the object from Office 365. You can do this by moving the user to an OU that is not synced with AADConnect. After that, run another AD Sync or wait until the next time the scheduled task runs. This sync will remove the user from Azure AD and flag the mailbox as "SoftDeleted". That puts the mailbox in the recycle bin, there it will stay for 30 days before it will be automatically deleted.

## Removing the mailbox from the recycle bin

In order to delete the user from the cloud recycle bin, you can use the following Powershell commands

<pre class="lang:ps decode:true ">Get-MsolUser -ReturnDeletedUsers - | FL UserPrincipalName,ObjectID

Remove-MsolUser -ObjectId  <GUID < -RemoveFromRecycleBin -Force</pre>

Because of the distributed nature of Office 365, it can take up to 15 minutes for the changes to replicate. Now we can hard delete the mailbox

<pre class="lang:ps decode:true ">Remove-Mailbox -Identity user@domain.com -PermanentlyDelete</pre>

## Finishing up

As a final step, move the user back to an OU that is synced. The next time the scheduled task runs, the user will be recreated in Azure AD and will appear in the Office 365 admin center.