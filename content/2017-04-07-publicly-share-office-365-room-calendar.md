---
title: Publicly share Office 365 room calendar
author: Maarten Van Driessen
type: post
date: 2017-04-07T12:43:29+00:00
url: /publicly-share-office-365-room-calendar/
categories:
  - Office 365
tags:
  - office365
  - powershell

---
A customer asked me if it was possible to have a room mailbox automatically accept meeting requests from external parties. They would also like to publish the calendar of that specific room publicly.

# Accept meetings from external parties

Let's start with the first question. By default, resource mailboxes only accept requests from internal senders. As you might guess, you can't change this behavior through the GUI, Powershell to the rescue!

Since I didn't know the cmdlet that would let me change this behavior, the first thing I did was look for all "Calendar cmdlets". After connecting to the Office 365 PowerShell, I ran this command

<pre class="lang:ps decode:true">get-command *calendar*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-CalendarDiagnosticAnalysis                     1.0        tmp_xse1ew1u.eif
Function        Get-CalendarDiagnosticLog                          1.0        tmp_xse1ew1u.eif
Function        Get-CalendarDiagnosticObjects                      1.0        tmp_xse1ew1u.eif
Function        Get-CalendarNotification                           1.0        tmp_xse1ew1u.eif
Function        Get-CalendarProcessing                             1.0        tmp_xse1ew1u.eif
Function        Get-MailboxCalendarConfiguration                   1.0        tmp_xse1ew1u.eif
Function        Get-MailboxCalendarFolder                          1.0        tmp_xse1ew1u.eif
Function        Set-CalendarNotification                           1.0        tmp_xse1ew1u.eif
Function        Set-CalendarProcessing                             1.0        tmp_xse1ew1u.eif
Function        Set-MailboxCalendarConfiguration                   1.0        tmp_xse1ew1u.eif
Function        Set-MailboxCalendarFolder                          1.0        tmp_xse1ew1u.eif</pre>

Seems like there are a few cmdlets concerning calendars, good info for the second question! The Get-CalendarProcessing cmdlet looks promising, let's try it out!

<pre class="lang:ps mark:35 decode:true">get-mailbox "test room" | Get-CalendarProcessing | fl


RunspaceId                          : 9f1b6e5d-09a6-40d9-9b83-8006a50d4284
AutomateProcessing                  : AutoUpdate
AllowConflicts                      : False
BookingWindowInDays                 : 180
MaximumDurationInMinutes            : 1440
AllowRecurringMeetings              : True
EnforceSchedulingHorizon            : True
ScheduleOnlyDuringWorkHours         : False
ConflictPercentageAllowed           : 0
MaximumConflictInstances            : 0
ForwardRequestsToDelegates          : True
DeleteAttachments                   : True
DeleteComments                      : True
RemovePrivateProperty               : True
DeleteSubject                       : True
AddOrganizerToSubject               : True
DeleteNonCalendarItems              : True
TentativePendingApproval            : True
EnableResponseDetails               : True
OrganizerInfo                       : True
ResourceDelegates                   : {}
RequestOutOfPolicy                  : {}
AllRequestOutOfPolicy               : False
BookInPolicy                        : {}
AllBookInPolicy                     : True
RequestInPolicy                     : {}
AllRequestInPolicy                  : False
AddAdditionalResponse               : False
AdditionalResponse                  :
RemoveOldMeetingMessages            : True
AddNewRequestsTentatively           : True
ProcessExternalMeetingMessages      : False
RemoveForwardedMeetingNotifications : False
MailboxOwnerId                      : test room
Identity                            : test room
IsValid                             : True
ObjectState                         : Changed</pre>

As you can see on the highlighted line, this is exactly the property we were looking for. Let's change it so we get the desired behavior. In the get-command output, I saw a cmdlet Set-CalendarProcessing, this seems like the right one.

<pre class="lang:ps decode:true ">Get-Mailbox "test room" | Set-CalendarProcessing -ProcessExternalMeetingMessages $true</pre>

This change will only affect new meeting requests, requests that have already been refused won't be automatically accepted.

# Publish calendar publicly

In the cmdlets we got earlier, there wasn't really one that stood out as a "possible match" so let's look at the attributes of the calendar itself. In essence, the calendar is just a folder inside of a mailbox object. Let's query that folder directly.

<pre class="lang:ps decode:true">Get-MailboxCalendarFolder testroom@domain.com:\calendar | fl


RunspaceId                      : 
Identity                        : test room:\calendar
PublishEnabled                  : False
PublishDateRangeFrom            : ThreeMonths
PublishDateRangeTo              : ThreeMonths
DetailLevel                     : AvailabilityOnly
SearchableUrlEnabled            : False
PublishedCalendarUrl            :
PublishedICalUrl                :
CalendarSharingOwnerSmtpAddress :
CalendarSharingPermissionLevel  : Null
SharingLevelOfDetails           : None
SharingPermissionFlags          : None
SharingOwnerRemoteFolderId      : AAA=
IsValid                         : True
ObjectState                     : Changed</pre>

That's everything we need and more! As you can see, we can set the **PublishEnabled** attribute to true but we can do so much more. You can choose the detail level and even set how far back and forth the published calendar needs to go.

Let's publish the calendar and run the Get-MailboxCalendarFolder cmdlet again to get the URL.

<pre class="lang:ps decode:true ">Set-MailboxCalendarFolder testroom@domain.com:\calendar -PublishEnabled $true

Get-MailboxCalendarFolder testroom@domain.com:\calendar | fl


RunspaceId                      : 
Identity                        : test room:\calendar
PublishEnabled                  : True
PublishDateRangeFrom            : ThreeMonths
PublishDateRangeTo              : ThreeMonths
DetailLevel                     : FullDetails
SearchableUrlEnabled            : False
PublishedCalendarUrl            : http://outlook.office365.com/owa/calendar/abc@domain.com/xyz/calendar.html
PublishedICalUrl                : http://outlook.office365.com/owa/calendar/abc@domain.com/xyz/calendar.ics
ExtendedFolderFlags             : ExchangePublishedCalendar
CalendarSharingOwnerSmtpAddress :
CalendarSharingPermissionLevel  : Null
SharingLevelOfDetails           : None
SharingPermissionFlags          : None
SharingOwnerRemoteFolderId      : AAA=
IsValid                         : True
ObjectState                     : Changed</pre>

All done! Now you can browse to the URL and verify everything is being displayed as you'd expect.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar.png" target="_blank"><img loading="lazy" class="aligncenter wp-image-192 size-large" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar-1024x631.png?resize=1024%2C631" alt="" width="1024" height="631" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar.png?resize=1024%2C631&ssl=1 1024w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar.png?resize=300%2C185&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar.png?resize=768%2C473&ssl=1 768w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2017/04/published-calendar.png?w=1519&ssl=1 1519w" sizes="(max-width: 1000px) 100vw, 1000px" data-recalc-dims="1" /></a>