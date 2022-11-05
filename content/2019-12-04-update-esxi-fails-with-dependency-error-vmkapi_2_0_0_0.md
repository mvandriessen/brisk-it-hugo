---
title: Update ESXi fails with dependency error vmkapi_2_0_0_0
author: Maarten Van Driessen
type: post
date: 2019-12-04T14:30:15+00:00
url: /update-esxi-fails-with-dependency-error-vmkapi_2_0_0_0/
categories:
  - VMware
tags:
  - esxcli
  - ESXi
  - update
  - VMware

---
In the past few weeks, I've been in the process of updating several standalone ESXi hosts to 6.5. As you would expect, everything went smooth up to a certain point. Several hosts started failing and throwing this error:

<pre class="">VIB LSI_bootbank_scsi-megaraid-perc9_6.901.55.00-1OEM.500.0.0.472560 requires vmkapi_2_0_0_0, but the requirement cannot be satisfied within the ImageProfile.
VIB LSI_bootbank_scsi-mpt3sas_04.00.00.00.1vmw-1OEM.500.0.0.472560 requires com.vmware.driverAPI-9.2.0.0, but the requirement cannot be satisfied within the ImageProfile.</pre>

<p class="">
  The entire error is shown below.
</p>

<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?ssl=1" rel="noopener noreferrer"><img loading="lazy" class="aligncenter wp-image-330 size-large" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?resize=1024%2C204&#038;ssl=1" alt="" width="1024" height="204" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?resize=1024%2C204&ssl=1 1024w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?resize=300%2C60&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?resize=768%2C153&ssl=1 768w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?resize=1536%2C306&ssl=1 1536w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2019/12/2019-12-04-14_09_36-Window.png?w=1713&ssl=1 1713w" sizes="(max-width: 1000px) 100vw, 1000px" data-recalc-dims="1" /></a>

At first, I thought it was something with the image I was using. That's why I also tried with the vanilla VMware image. Sadly, the same error popped up. Trying different zip files also did not solve the issue. I began looking into how I could make vmkapi 2.0 available on the host. Turns out that there's not a whole lot written about this.

I started looking at the command again when I remembered there's also another way I could install the update. When using esxcli to update, you can also use esxcli software profile install command. The update command updates existing VIBs with the ones that are in the specified profile, all the other VIBs aren't touched. While the esxcli software profile install command installs all the VIBs present in the image profile, it will also remove any other VIBs installed on the server.

When I ran the install command everything proceeded without any issues. Here's the command I used;

<pre class="lang:ps decode:true">esxcli software profile install -p DellEMC-ESXi-6.5U3-14320405-A03 -d /vmfs/volumes/datastore/ISO/VMware-VMvisor-Installer-6.5.0.update03-14320405.x86_64-DellEMC_Customized-A03.zip --ok-to-remove</pre>

You can also add the -dry-run switch to get a list of VIBs that will be removed, before you actually change anything.