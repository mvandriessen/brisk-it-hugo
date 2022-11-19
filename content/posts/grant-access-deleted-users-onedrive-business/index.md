---
title: How to grant access to deleted user’s Onedrive for Business library
author: Maarten Van Driessen
type: post
date: 2016-11-08T11:19:14+00:00
url: /grant-access-deleted-users-onedrive-business/
categories:
  - Office 365
tags:
  - office365
  - onedrive for business
  - sharepoint

---
A couple of weeks ago we had an issue which resulted in some users getting deleted from Office 365. Because of a sync tool we use, we had to recreate the user accounts instead of restoring them. As a result of this (different SID), the new users also got a new Onedrive for Business library and could no longer access their existing library.

## Listing all disabled user profiles

In the Office 365 admin center, go to the SharePoint admin center and click _**User profiles**_ and _**Manage User Profiles.**_

{{< figure src="./images/User-profiles.png" alt="user profiles">}}

While using various searches, I noticed that every user had **_i:0#.f|membership_** in front of their UPN. So I decided to use that as the search term, turns out this lists all users. After changing the view to _**Profiles missing from import**_, I got a list of all the disabled users.

{{< figure src="./images/deleted-profiles.png" alt="deleted profiles">}}

## Changing Onedrive for Business library permissions

Now that we can see the profiles, we can change the permissions. Click the three dots behind the profile and select _**Manage Site collection owners**_

{{< figure src="./images/edit-site-collection-owner.png" alt="edit site collection owner">}}

There you can set the new user as a _**Site collection administrator**_. Once you click OK, the users can access the old library using the web version of Onedrive for Business.

{{< figure src="./images/set-site-collection-owner.png" alt="set-site-collection-owner">}}

There is one caveat to all this, the old profiles will be deleted after 30 days, as part of the Office 365 automated cleanup.