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

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png" target="_blank"><img loading="lazy" class="aligncenter  wp-image-128" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png?resize=963%2C141" alt="user-profiles" width="963" height="141" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png?w=1134&ssl=1 1134w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png?resize=300%2C44&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png?resize=768%2C112&ssl=1 768w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/User-profiles.png?resize=1024%2C150&ssl=1 1024w" sizes="(max-width: 963px) 100vw, 963px" data-recalc-dims="1" /></a>

While using various searches, I noticed that every user had **_i:0#.f|membership_** in front of their UPN. So I decided to use that as the search term, turns out this lists all users. After changing the view to _**Profiles missing from import**_, I got a list of all the disabled users.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png" target="_blank"><img loading="lazy" class="aligncenter size-full wp-image-125" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png?resize=1467%2C142" alt="deleted-profiles" width="1467" height="142" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png?w=1467&ssl=1 1467w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png?resize=300%2C29&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png?resize=768%2C74&ssl=1 768w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/deleted-profiles.png?resize=1024%2C99&ssl=1 1024w" sizes="(max-width: 1000px) 100vw, 1000px" data-recalc-dims="1" /></a>

## Changing Onedrive for Business library permissions

Now that we can see the profiles, we can change the permissions. Click the three dots behind the profile and select _**Manage Site collection owners. **_

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/edit-site-collection-owner.png" target="_blank"><img loading="lazy" class="aligncenter size-full wp-image-126" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/edit-site-collection-owner.png?resize=571%2C150" alt="edit-site-collection-owner" width="571" height="150" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/edit-site-collection-owner.png?w=571&ssl=1 571w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/edit-site-collection-owner.png?resize=300%2C79&ssl=1 300w" sizes="(max-width: 571px) 100vw, 571px" data-recalc-dims="1" /></a>

There you can set the new user as a _**Site collection administrator**_. Once you click OK, the users can access the old library using the web version of Onedrive for Business.

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/set-site-collection-owner.png" target="_blank"><img loading="lazy" class="aligncenter  wp-image-127" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/set-site-collection-owner.png?resize=528%2C346" alt="set-site-collection-owner" width="528" height="346" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/set-site-collection-owner.png?w=693&ssl=1 693w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2016/11/set-site-collection-owner.png?resize=300%2C197&ssl=1 300w" sizes="(max-width: 528px) 100vw, 528px" data-recalc-dims="1" /></a>

There is one caveat to all this, the old profiles will be deleted after 30 days, as part of the Office 365 automated cleanup.