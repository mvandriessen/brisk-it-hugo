---
title: VCSA upgrade – deployment sizes missing
author: Maarten Van Driessen
type: post
date: 2020-06-11T10:59:08+00:00
url: /vcsa-upgrade-deployment-sizes-missing/
categories:
  - VMware
tags:
  - logs
  - update
  - VMware

---
Everyone has done VCSA upgrades dozens of times, but every now and then you come across something that you haven't seen before. Today, this was the case while I was doing an upgrade.

When you get to the deployment size selection, in the first stage of the upgrade, I noticed I was unable to select anything smaller than "Medium&#8221; size.<a href="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?ssl=1" target="_blank" rel="noopener noreferrer"><img loading="lazy" class="aligncenter wp-image-336 size-large" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?resize=1024%2C607&#038;ssl=1" alt="" width="1024" height="607" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?resize=1024%2C607&ssl=1 1024w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?resize=300%2C178&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?resize=768%2C455&ssl=1 768w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/medium-size.png?w=1134&ssl=1 1134w" sizes="(max-width: 1000px) 100vw, 1000px" data-recalc-dims="1" /></a>

Changing the storage size also didn't make the Tiny or small sizes available. While I was doing this, I happened to be talking to <a href="https://twitter.com/HerremansJens" target="_blank" rel="noopener noreferrer">Jens Herremans</a> (Check out his [blog][1], it's awesome!). He told me to check the size of the logs partition on the source VM.

## Checking log sizes

After opening an SSH session to the source VM and changing to the /var/log/vmware folder, I ran the following command to get a list of all files and their sizes.

<pre class="lang:sh decode:true ">ls -lahR  < < size.txt</pre>

I write the output to a file to make it easier to review, also this avoids having to scroll up and having parts capped off. Next, review the file with your favorite editor. You'll see a list of all the folders, their total size and the individual files with their size. For me the big directories are vpxd, sca, sso, vdcs and vsphere-client.

Every file will also have a timestamp that makes it easy to verify if you still need the logs or not. If you wish to keep the, you can easily move them to a datastore or copy them locally. In this particular environment, we had no need for any of the older log files. I could just go in and rm -rf all the archived logs. For example in the /var/log/vmware/sca folder, the following command will remove all the archived logs but keep the one currently in use

<pre class="lang:sh decode:true ">rm -rf sca.log.*</pre>

# 

After the cleanup, I ran the upgrade assistant again and was able to select the tiny deployment size.[<img loading="lazy" class="aligncenter size-full wp-image-337" src="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/tiny-size.png?resize=835%2C532&#038;ssl=1" alt="" width="835" height="532" srcset="https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/tiny-size.png?w=835&ssl=1 835w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/tiny-size.png?resize=300%2C191&ssl=1 300w, https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/tiny-size.png?resize=768%2C489&ssl=1 768w" sizes="(max-width: 835px) 100vw, 835px" data-recalc-dims="1" />][2]

 [1]: https://cloud-duo.com/
 [2]: https://i0.wp.com/www.brisk-it.net/wp-content/uploads/2020/06/tiny-size.png?ssl=1